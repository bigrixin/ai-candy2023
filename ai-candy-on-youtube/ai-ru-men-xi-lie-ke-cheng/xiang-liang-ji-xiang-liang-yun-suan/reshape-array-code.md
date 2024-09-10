# Reshape array code

{% code fullWidth="true" %}
```python
def reshape1():
    # (2, 3, 2, 4, 4, 5) to (12, 4, 4, 5)
    print("==== reshape1 result======")
 
    # 定义数组的形状
    orignal_shape = (2, 3, 2, 4, 4, 5)

    # 生成形状为 (2, 3, 2, 4, 4, 5) 的随机整数数组，值在 0 到 9 之间
    source_data = np.random.randint(0, 10, size=orignal_shape)
    print('------old shape---' +str(source_data.shape) )
    print(source_data)  #  (2, 3, 2, 4, 4, 5)

    old_data = np.array(source_data)   
    ### (2*3*2 = 12)
    new_data_size = old_data.shape[0] * old_data.shape[1] * old_data.shape[2]
    reshaped_data = old_data.reshape(new_data_size, 4, 4, 5)  ## (12, 4, 4, 5)
    print('------new shape----'+str(reshaped_data.shape))
    print(reshaped_data)

############################################################################

def reshape2():
    # (2,3,5) to (2,1,3,5)
    print("==== reshape2 result======")

    old_shape = (2,3,5)
    old_data =np.random.randint(0, 10, size=old_shape)
    print('------old shape---' +str(old_data.shape) )
    print(old_data)
    new_shape=old_data.reshape(2,1,3,5)
    print('------new shape----'+str(new_shape.shape))
    print(new_shape)
    
############################################################################
 
if __name__ == "__main__":  
    reshape2()
```
{% endcode %}

\==== reshape2 result======

\------old shape---(2, 3, 5)&#x20;

\[\[\[4 4 7 4 6] \[5 7 1 5 9] \[5 2 0 9 6]]

\[\[5 9 7 9 1] \[7 9 1 8 8] \[9 2 8 2 4]]]&#x20;

\------new shape----(2, 1, 3, 5)&#x20;

\[\[\[\[4 4 7 4 6] \[5 7 1 5 9] \[5 2 0 9 6]]]

\[\[\[5 9 7 9 1] \[7 9 1 8 8] \[9 2 8 2 4]]]]
