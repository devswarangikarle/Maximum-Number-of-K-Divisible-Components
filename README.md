# Maximum-Number-of-K-Divisible-Components

There is an undirected tree with n nodes labeled from 0 to n - 1. You are given the integer n and a 2D integer array edges of length n - 1, where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree.

You are also given a 0-indexed integer array values of length n, where values[i] is the value associated with the ith node, and an integer k.

A valid split of the tree is obtained by removing any set of edges, possibly empty, from the tree such that the resulting components all have values that are divisible by k, where the value of a connected component is the sum of the values of its nodes.

Return the maximum number of components in any valid split.

class Solution:
    def maxKDivisibleComponents(self, n: int, edges: List[List[int]], values: List[int], k: int) -> int:
        # Build the adjacency list for the tree
        tree = defaultdict(list)
        for a, b in edges:
            tree[a].append(b)
            tree[b].append(a)
        
        # Initialize variables
        visited = set()
        self.components = 0  # Track the number of valid components
        
        def dfs(node):
            # Mark the node as visited
            visited.add(node)
            # Start the subtree sum with the node's value
            subtree_sum = values[node]
            
            for neighbor in tree[node]:
                if neighbor not in visited:
                    # Recursively calculate the subtree sum
                    child_sum = dfs(neighbor)
                    # If the child's sum is divisible by k, it's a valid component
                    if child_sum % k == 0:
                        self.components += 1
                    else:
                        subtree_sum += child_sum
            
            return subtree_sum
        
        # Perform DFS from an arbitrary root (node 0)
        total_sum = dfs(0)
        # If the entire tree's sum is divisible by k, increment components
        if total_sum % k == 0:
            self.components += 1
        
        return self.components
