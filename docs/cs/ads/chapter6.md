# Backtracking

## Minimax Strategy

在类似下棋的搜索中，使用 Minimax 策略，最大化自己的利益，同时最小化对手的利益。

### &alpha;-&beta; Pruning

每个节点维护 \( \alpha \) 和 \( \beta \) 两个值，分别表示当前子树收益的上界和下界。

在搜索过程中，如果继续搜索对当前节点没有意义，可以进行剪枝。