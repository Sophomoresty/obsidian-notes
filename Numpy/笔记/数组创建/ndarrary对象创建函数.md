## np.ones()
默认数据类型为 `float64 `
![[numpy_思维导图.md_Attachments/Pasted Image 20250225132807_517.png]]
## np.ones_like()

创建的数组和目标数组 `a` 
- 形状相同
- 数据类型相同
![[numpy_思维导图.md_Attachments/cropped_Pasted Image 20250225132958_989]]

## np.empty ()
用于创建一个指定形状（shape）、数据类型（dtype）但不初始化元素值的数组。

不初始化意味着数组中的元素值是未定义的，它们可能是任何值, 默认 `dtype` 为 `float64`

## np.arrange()

- arange(stop)
- arange(start, stop)
- arange(start, stop, step) 

|  Name   | arg  |
| :-----: | :--: |
| arrange | stop |
|         |      |
|         |      |

	## np.linspace
numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None, axis=0, `*`, device=None)
