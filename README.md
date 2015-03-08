# 算法与数据结构

* 基本排序
  * [heap sort 堆排序](#heap_sort) 
  * [qucik sort 快速排序](#heap_sort) 
  * [insert sort 插入排序](#heap_sort) 
  * [merge sort 合并排序](#heap_sort) 
  * [radix sort 基数排序](#heap_sort) 
  * [count sort 计数排序](#heap_sort) 


## <a name="heap_sort">堆排序</a> [&#8593;](#heap_sort)
**描述:** 堆排序是基于比较的的排序算法。堆排序可以看作是改良的选择排序,它采用堆数据结构,每次从待排序的序列中选出最大或者最小的元素。

(二叉)堆数据结构本质上是建立在一个数组对象上的,可以把它看作是完全二叉树,如下图所示。树上的每一个节点对应于数组上的一个元素。该数组A有2个属性,数组长度A.length和堆长度A.heap-size,其中A.length>=A.heap_size。堆的第一个元素存储在数组下标为1的位置上A[1],堆的存储下标范围是1...heap-size。

堆树的根节点是A[1],给出一个节点下标很容易得出其左右孩子节点和父节点:

```
parent(i)
  return i/2;

left(i)
  return 2*i;

right(i)
  return 2*i + 1;
```

堆分为大堆和小堆,在大堆中每个节点i满足A[parent(i)] > A[i],根节点A[1]为最大值；小堆则相反,每个节点i满足A[parent(i)] < A[i],根节点A[1]为最小值。

堆排序分为两步:建堆和取最大值。首先把待排序的序列建堆(大堆),然后取最大值A[1],把其与堆中最后一个元素交换,把堆的长度减1,这时破坏了堆,于是重新建堆,如此循环直至堆大小为1,此时数组A就排好了序。下面是堆排序代码和演示图(图源来自wiki):

![堆排序演示图](http://upload.wikimedia.org/wikipedia/commons/4/4d/Heapsort-example.gif)

```
//时间复杂度为T(n) = n * T(maxHeapify) = n * log(n)
void buildHeapSort(int a[],int length) 
{
  int tmp,i; 
  for(i=1;i<length - 1;i++)
  {
    tmp = a[1];//a[1]是最大的一个数
    a[1] = a[length - i];//把a[1]最大的树和最后一个交换，再调用maxHeapify进行堆树调整，到循环结束即达到排序的序列
    a[length - i] = tmp;
    maxHeapify(a,1,length - i);
  }
}
//时间复杂度为树的高度T(n) = log(n)
void maxHeapify(int a[],int index,int length)
{
  int tmp,l,r;
  l = 2*index;
  r = 2*index + 1;
  printf("l=%d,r=%d,i=%d\n",l,r,index);
  if(r<length&&l<length)
  {
    if((a[l]>a[r]) && a[index] < a[l])
    {
      tmp = a[index];
      a[index] = a[l];
      a[l] = tmp;
      maxHeapify(a,l,length);
    }
    if((a[l]<a[r]) && a[index] < a[r])
    {
      tmp = a[index];
      a[index] = a[r];
      a[r] = tmp;
      maxHeapify(a,r,length);
    }
  }
  else if(r>=length&&l<length)
    if(a[index] < a[l])
    {
      tmp = a[index];
      a[index] = a[l];
      a[l] = tmp;
      maxHeapify(a,l,length);
    }
}

//创建最大堆树
void buildMaxHeap(int a[],int length)
{
  int mid = (length)/2;
  int i;
  for(i = mid; i >= 1; i--)
  {
    maxHeapify(a,i,length);
  }
}
```

**时间复杂度:**从上面分析可知T(buildHeapSort(n)) = n * T(maxHeapify) = n * log(n)。

**应用:**堆排序可应用于优先级队列,在linux中其进程调度使用的调度数据结构是位图bit array,查找的时间复杂度为O(1)。
