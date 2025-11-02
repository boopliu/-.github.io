# Huffman树与Huffman编码

已知26个字母具有不同的使用频率（可预设，也可手工输入），该程序实现了编程构建huffman树，生成huffman编码。

## 参考博客
https://oi-wiki.org/ds/huffman-tree/

## 网站推荐
霍夫曼树生成器
https://www.csfieldguide.org.nz/en/interactives/huffman-tree/
可貌似不能加换行符？？？

<img width="1406" height="402" alt="Image" src="https://github.com/user-attachments/assets/126b7243-8a30-4164-9afd-3d941fe449a4" />

## 题目参考图片

<img width="840" height="720" alt="Image" src="https://github.com/user-attachments/assets/adba11f4-7ced-47ba-ad20-495fb0135164" />

## 代码

```c
#include<stdio.h>
#include<stdlib.h>
#include <string.h>
const int N = 26;

struct Hnode{
	int weight;
	struct Hnode *lchild, *rchild;
}; 

/**
 * 创建哈夫曼树
 * @param arr 权值数组，每个元素表示一个字符的频数
 * @param n   字符个数
 * @return    返回哈夫曼树的根节点指针
 */
struct Hnode* creatHtree(int arr[], int n){
	struct Hnode* Hforest[N];
	struct Hnode* root = NULL;
	for(int i = 0; i < n; i++){
		struct Hnode* tmp = (struct Hnode*)malloc(sizeof(Hnode));
		tmp->weight = arr[i];
		tmp->lchild = tmp->rchild = NULL; 
		Hforest[i] = tmp;
	}
	
	for(int i = 1; i < n; i++){
		 
		int minn = -1, minn2 = -1;
		for(int j = 0; j < n; j++){
			if(Hforest[j] != NULL){
				if(minn == -1){
					minn = j;
					continue;
				} else {
					minn2 = j;
					break;
				}
			}
		}
		 
		for(int j = minn2; j < n; j++){
			if(Hforest[j] == NULL)continue;
			if(Hforest[j]->weight < Hforest[minn]->weight){
				minn2 = minn;
				minn = j;
			} else if(Hforest[j]->weight < Hforest[minn2]->weight){
				minn2 = j;
			}
		}
		
		// 不要加 struct Hnode*，直接赋值,否则root一直为NULL 
		root = (struct Hnode*)malloc(sizeof(Hnode));
		root->weight = Hforest[minn]->weight + Hforest[minn2]->weight;
		root->lchild = Hforest[minn];
		root->rchild = Hforest[minn2];
		
		Hforest[minn] = root;
		Hforest[minn2] = NULL;
	}
	return root;
}
/**
 * 递归生成哈夫曼编码
 * @param root 哈夫曼树根节点指针
 * @param len  当前路径长度（编码长度）
 * @param arr  保存当前路径的数组（0表示左，1表示右）
 * @return     无返回值，直接在控制台打印叶子节点的编码
 */
void getHtreecode(Hnode* root, int len, int arr[]){
	if(root == NULL)return;
	
	if(root->lchild == NULL && root->rchild == NULL){
		printf("结点为%d的字符编码为：", root->weight);
		for(int i = 0; i < len; i++){
			printf("%d", arr[i]);
		} 
		printf("\n");
		return;
	}
	
	arr[len] = 0;
	getHtreecode(root->lchild, len + 1, arr);
	
	arr[len] = 1;
	getHtreecode(root->rchild, len + 1, arr);
}

/**
 * 递归释放哈夫曼树内存
 * @param root 哈夫曼树根节点指针
 * @return     无返回值，释放所有节点占用的内存
 */
void freeHtree(struct Hnode* root){
    if(root == NULL) return;
    freeHtree(root->lchild);
    freeHtree(root->rchild);
    free(root);
}

int main(){
	int n;
	printf("请输入字母的个数：");
	scanf("%d", &n);
	int arr[n];
	
	for(int i = 0; i < n; i++){
		char c = 'A' + i;
		printf("请输入字母%c的频数:",c);
		scanf("%d", &arr[i]);
	} 
	struct Hnode* Htree = creatHtree(arr, n);
	int ans [N];
//	if(Htree == NULL)puts("FUCK");
	getHtreecode(Htree, 0, ans);
	freeHtree(Htree);
	return 0;
}
/*
测试样例：
6 
5 9 12 13 16 45 
*/
```
## 运行结果
与生成的Huffman树完全一致

<img width="759" height="505" alt="Image" src="https://github.com/user-attachments/assets/54039e5c-b635-4ac5-a042-35511e17dc49" />

