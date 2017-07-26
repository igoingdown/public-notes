## 2017.5.15

1. torch的第一个坑：==dot==操作的结果不是数学中的点积的结果。

    ```python
    data = [[-1, -2], [-4, -5]]
    tensor = torch.FloatTensor(data)
    np_array = np.array(data)
    print torch.mm(tensor, tensor), "\n", np.matmul(data,data)
    # 输出：  9  12
    #         24  33
    #         [torch.FloatTensor of size 2x2]
    # 
    #         [[ 9 12]
    #         [24 33]]

    print tensor.dot(tensor), "\n", np_array.dot(np_array)
    # 输出：  46.0 
    # 
    #         [[ 9 12]
    #         [24 33]]
    ```

2. list, np.array, torch.FloatTensor, torch.Variable的关系

    ==四者之间的关系是层层封装的！==
    ```python
    data = [[-1, -2], [-4, -5]]
    np_array = np.array(data)
    tensor = torch.FloatTensor(data)
    variable = torch.autograd.Variable(tensor)

    # 同理，也可以按反方向解封装
    tensor = variable.data
    np_array = tensor.numpy()
    data = np_array.tolist()
    ```


