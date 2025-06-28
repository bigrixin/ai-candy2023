---
description: 用于训练模型，以获得最佳参数
---

# 贝叶斯优化

{% code fullWidth="true" %}
```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torch.utils.data import DataLoader, Dataset
import numpy as np
from skopt import gp_minimize
from skopt.space import Real, Integer, Categorical
from skopt.utils import use_named_args
import time
import copy
from model import *



# 训练函数
def train_model(model, train_loader, val_loader, criterion, optimizer, device, num_epochs=5):
    model = model.to(device)
    best_val_acc = 0.0
    best_model = None
    
    for epoch in range(int(num_epochs)):
        model.train()
        running_loss = 0.0
        
        for inputs, labels in train_loader:
            inputs, labels = inputs.to(device), labels.to(device)
            
            optimizer.zero_grad()
            outputs = model(inputs, inputs)
            loss = criterion(outputs, labels)
            loss.backward()
            optimizer.step()
            
            running_loss += loss.item()
        
        # 验证
        val_acc = evaluate_model(model, val_loader, device)
        print(f'Epoch {epoch+1}/{num_epochs}, Loss: {running_loss/len(train_loader):.4f}, Val Acc: {val_acc:.4f}')
        
        if val_acc > best_val_acc:
            best_val_acc = val_acc
            best_model = copy.deepcopy(model)
    
    return best_model, best_val_acc

# 评估函数
def evaluate_model(model, data_loader, device):
    model.eval()
    correct = 0
    total = 0
    
    with torch.no_grad():
        for inputs, labels in data_loader:
            inputs, labels = inputs.to(device), labels.to(device)
            outputs = model(inputs, inputs)
            _, predicted = torch.max(outputs, 1)
            total += labels.size(0)
            correct += (predicted == labels).sum().item()
    
    return correct / total

# 定义超参数空间
dimensions = [
    # CNN参数
    Integer(16, 64, name='conv1_channels'),
    Integer(32, 128, name='conv2_channels'),
    Categorical([3, 5], name='kernel_size'),
    Categorical([True, False], name='use_leaky_relu'),
    Real(0.2, 0.7, name='dropout_rate'),
    
    # LSTM参数
    Integer(64, 256, name='lstm_hidden'),
    Integer(1, 3, name='lstm_layers'),
    
    # 融合模型参数
    Integer(128, 512, name='fc1_units'),
    
    # 训练参数
    Real(1e-4, 1e-2, prior='log-uniform', name='learning_rate'),
    Integer(16, 128, name='batch_size'),
    Categorical(['adam', 'sgd'], name='optimizer'),
    Real(0, 0.001, name='weight_decay')
]

# 目标函数 - 贝叶斯优化将尝试最小化此函数
@use_named_args(dimensions)
def objective(**params):
    start_time = time.time()
    
    # 打印当前测试的超参数组合
    print(f"Testing hyperparameters: {params}")
    
    # 创建模型，确保所有整数值参数转换为Python原生int
    model = Fusion_CNN2D_LSTM_Model(
        input_channel=5,
        conv1_channels=int(params['conv1_channels']),
        conv2_channels=int(params['conv2_channels']),
        kernel_size=int(params['kernel_size']),
        use_leaky_relu=params['use_leaky_relu'],
        dropout_rate=params['dropout_rate'],
        lstm_hidden=int(params['lstm_hidden']),
        lstm_layers=int(params['lstm_layers']),
        fc1_units=int(params['fc1_units'])
    )
    
    # 假设的设备配置
    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    
    # 假设的数据集和数据加载器
    train_dataset = MyDataset(torch.randn(1000, 5, 64, 64), torch.randint(0, 4, (1000,)))
    val_dataset = MyDataset(torch.randn(200, 5, 64, 64), torch.randint(0, 4, (200,)))
    train_loader = DataLoader(train_dataset, batch_size=int(params['batch_size']), shuffle=True)
    val_loader = DataLoader(val_dataset, batch_size=int(params['batch_size']))
    
    # 定义损失函数和优化器
    criterion = nn.CrossEntropyLoss()
    
    if params['optimizer'] == 'adam':
        optimizer = optim.Adam(model.parameters(), 
                             lr=params['learning_rate'],
                             weight_decay=params['weight_decay'])
    else:
        optimizer = optim.SGD(model.parameters(), 
                            lr=params['learning_rate'],
                            momentum=0.9,
                            weight_decay=params['weight_decay'])
    
    # 训练模型
    _, val_acc = train_model(model, train_loader, val_loader, criterion, optimizer, device, num_epochs=3)
    
    # 计算耗时
    elapsed_time = time.time() - start_time
    print(f"Validation accuracy: {val_acc:.4f}, Elapsed time: {elapsed_time:.2f}s")
    
    # 贝叶斯优化目标是最小化，所以返回1 - 准确率
    return 1 - val_acc

# 执行贝叶斯优化
def bayesian_optimization(n_calls=10):
    print("Starting Bayesian optimization...")
    result = gp_minimize(
        func=objective,
        dimensions=dimensions,
        n_calls=n_calls,
        random_state=42,
        verbose=True
    )
    
    print("\n贝叶斯优化完成!")
    print(f"最佳验证准确率: {1 - result.fun:.4f}")
    print("最佳超参数:")
    # 修改这里，正确获取参数名称
    for i, dimension in enumerate(result.space.dimensions):
        print(f"{dimension.name}: {result.x[i]}")
    
    return result

# 运行优化
if __name__ == "__main__":
    bayesian_optimization(n_calls=15)
```
{% endcode %}
