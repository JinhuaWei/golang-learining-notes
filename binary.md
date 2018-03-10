```
package main

import (
	"container/list"
	"errors"
	"fmt"
)

var (
	errTreeNil        = errors.New("tree is null")
	errTreeIndexExist = errors.New("tree index is existed")
)

type BSTree struct {
	root *Node
}
type Node struct {
	lchild *Node
	rchild *Node
	index  int
	data   int
}

func (tree *BSTree) Insert(index int, data int) error {
	//当前树为空
	if tree.root == nil {
		root := &Node{lchild: nil, rchild: nil, index: index, data: data}
		tree.root = root
		return nil
	}
	//将节点放入对应位置
	node := tree.root //why here need golobal variable
	for {
		if node.index == index {
			return errTreeIndexExist
		}
		//找到节点，插入数据
		if node.index > index {
			if node.lchild == nil {
				node.lchild = &Node{lchild: nil, rchild: nil, index: index, data: data}
				return nil
			} else {
				node = node.lchild
				break
			}
		}
		if node.index < index {
			if node.rchild == nil {
				node.rchild = &Node{lchild: nil, rchild: nil, index: index, data: data}
				return nil
			} else {
				node = node.rchild
				break
			}
		}
	}
	return nil
}

//中序遍历树，并根据钩子函数处理数据
func (tree *BSTree) Midtraverse(handle func(interface{}) error) error {
	//func (tree *BSTree) Midtraverse() error {
	if tree.root == nil {
		return errTreeNil
	}
	l := list.New()
	//node := tree.root
	//入队、压栈
	l.PushBack(tree.root)
	for {
		e := l.Front()
		if e == nil {
			break
		}
		node := e.Value.(*Node)
		l.Remove(e)

		if err := handle(node); err != nil {
			fmt.Printf("index:%d err:%s", node.index, err)
		}

		if node.lchild != nil {
			l.PushBack(node.lchild)
		}
		if node.rchild != nil {
			l.PushBack(node.rchild)
		}
	}
	return nil
}

func handled(node interface{}) error {
	fmt.Printf("node.index=%d node.(Node)data=%d\n", node.(*Node).index, node.(*Node).data)
	return nil
}

func main() {
	tree := &BSTree{}
	tree.Insert(3, 6)
	tree.Insert(4, 6)
	tree.Midtraverse(handled)
}
```
