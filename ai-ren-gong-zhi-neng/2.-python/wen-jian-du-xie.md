# 文件读写



{% code fullWidth="true" %}
```
with open(json_file, 'r', encoding="utf-8") as f:
    json_string = f.read()
```
{% endcode %}

{% code fullWidth="true" %}
```
with open(output_file_name, 'a+', encoding="utf-8") as f:
    f.write(cleared_text)
```
{% endcode %}

with open, 无需close

{% code fullWidth="true" %}
```
    fin = open(input_file_name, 'r', encoding="utf-8")
    fout = open(output_file_name, 'w+', encoding="utf-8")
    
    for line in fin:
        for word in delete_word_list:
            line = line.replace(word, '')
        fout.write(line)
    
    fin.close()
    fout.close()
```
{% endcode %}

{% code fullWidth="true" %}
```
// save to text file
    best_result_file = os.path.join(model_dir, f"bayesian_optimization_results_{method}.txt")
    with open(best_result_file, "w", encoding="utf-8") as f:
        f.write(f"Best validation accuracy: {1 - result.fun:.4f}\n")  # Maximize the accuracy (Accuracy)
        f.write("Optimal hyperparameters:\n")
        for param, value in best_params.items():
            f.write(f"{param}: {value}\n")



// read text file
def load_best_params(file_path=bayesian_optimization_results_A.txt):
    best_params = {}
    with open(file_path, "r", encoding="utf-8") as f:
        lines = f.readlines()
        # 跳过前两行（标题和空行）
        for line in lines[2:]:
            if ":" in line:
                param, value = line.strip().split(": ", 1)
                # 转换参数类型（根据实际情况调整）
                if param in ["conv1_channels", "conv2_channels", "kernel_size", "lstm_hidden", "lstm_layers", "fc1_units", "batch_size"]:
                    best_params[param] = int(value)
                elif param in ["dropout_rate", "learning_rate", "weight_decay"]:
                    best_params[param] = float(value)
                elif param in ["use_leaky_relu"]:
                    best_params[param] = True if value == "True" else False
                elif param in ["optimizer"]:
                    best_params[param] = value
    return best_params

```
{% endcode %}

{% code fullWidth="true" %}
```
// save to pth and pkl file

    # After the training is completed, save the best model.
    if best_model_state and model_dir:
        model_path = os.path.join(model_dir, f"best_model_{best_val_acc:.4f}.pth")
        torch.save(best_model_state, model_path)
        
    # result save to pkl file
    optimization_result_file = os.path.join(model_dir, "bayesian_optimization_result.pkl")
    with open(optimization_result_file, "wb") as f:
        pickle.dump(result, f)
    print(f"The optimized results have been successfully saved to: {optimization_result_file}")   
        
```
{% endcode %}



* r : 读取文件，若文件不存在则会报错
* w: 写入文件，若文件不存在则会先创建再写入，会覆盖原文件
* a : 写入文件，若文件不存在则会先创建再写入，但不会覆盖原文件，而是追加在文件末尾
* rb,wb：分别于r,w类似，但是用于读写二进制文件
* r+ : 可读、可写，文件不存在也会报错，写操作时会覆盖
* w+ : 可读，可写，文件不存在先创建，会覆盖
* a+ ：可读、可写，文件不存在先创建，不会覆盖，追加在末尾
