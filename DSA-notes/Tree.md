- Binary tree is a *Non linear* and *hierarchical* data structure where each node has at most two children.
- The two children are know as *left-child* and *right-child*
- To make a tree in c++ we can made the blueprint of the node in either a *class* or *struct*.

## Structure of Binary tree in c++
![[Binary-Tree-Representation.webp]]

## Terminologies in Binary Tree
- **Node:** Its the *blueprint* of a node contains, a *data-type*, *left pointer* and *right pointer*.
- **Root:** Root is the top most node in a tree.
- **Parent Node:** Every node which have a *left-child* and a *right-child* is know as *parent node*.
- **Child node:** A node which is a descendant of another node(*parent*).
- **Leaf node:** A node that is does not have any *children* or both *children are null*.
- **Internal Node:** A node that has at least one *child*. This includes all nodes except the *root* and the *leaf* nodes.
- **Depth of a Node:** The number of *edges* from a specific node to the root node. The depth of the *root* node is zero.
- **Height of a Binary Tree:** The number of nodes from the *deepest leaf* to the *root node*
 ![[Terminologies-in-Binary-Tree-in-Data-Structure_1.webp]]

## Properties of Binary Trees
- The maximum number of nodes at level *L* of binary tree is *2L*.
- The maximum number of nodes in a binary tree of height *H* is *2H-1*.
- Total number of leaf nodes in binary tree = total number of nodes with 2 children + 1.
- In a binary tree with *N* nodes, the minimum possible height or the minimum number of levels is *Log2(n+1)*
- A binary tree with *L* leaves had at least *|Log2L| = 1* levels.


## Types of Trees
1. Binary Search Tree:

- In Binary search tree every right *children* is greater than its parent node. And the left *children* is small than its parent.
 ![[BST.webp]]
## Types of Traversals

Normally there are many traversal algorithm are there for trees but i tried some of the basics
### In-order
  In *in-order* its go to left children and print its value then go to right children.*In-order Code:-*
	 *Code:-*
```c++
	void inOrderHelper(Node* node){
            if(node != nullptr){
                inOrderHelper(node -> left);
                cout << node -> data << " ";
                inOrderHelper(node -> right);
            }
        }
```
### Pre-Order
In *pre-order* its first print the value of node and then go to left children and then to right children.
	*Code:-*
```c++
	void preOrderHelper(Node* node){
            if(node != nullptr){
                cout << node -> data << " ";
                preOrderHelper(node -> left);
                preOrderHelper(node -> right);
            }
        }
```
### Post-Order
 In *Post-Order* its first go to left children then to the right children and then prints the value of the node.
	*Code:-*
```c++
	 void postOrderHelper(Node* node){
            if(node != nullptr){
                postOrderHelper(node -> left);
                postOrderHelper(node -> right);
                cout << node -> data << " ";
            }
        }
```
### Breadth First
In *Breadth First* its travels level by level. Before moving to the next level its clear all the nodes in the current level. We can achieve this by using queue.
	*Code:-*
```c++
	 void BFT(){
            if(root == nullptr) return;
    
            queue<Node*> q;

            q.push(root);

            while(!q.empty()){
                Node* currentNode = q.front();
                cout << currentNode -> data << " ";
                q.pop();

                if(currentNode -> left) q.push(currentNode -> left);
                if(currentNode -> right) q.push(currentNode -> right );
            }
        }
```

