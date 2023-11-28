# Tensor

## Tensor 生成

```c++
// 栈上对象：需同时指定类型和Layout，layout必须是静态shape
Tensor make_tensor<T>(Layout layout);

// 堆上对象：需指定pointer和Layout，layout可动可静
Tensor make_tensor(Pointer pointer, Layout layout);
 
// 栈上对象，tensor的layout必须是静态的
Tensor make_tensor_like(Tensor tensor); 

// 栈上对象，tensor的layout必须是静态的
Tensor make_fragment_like(Tensor tensor);
```