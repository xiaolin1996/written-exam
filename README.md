# 笔试题  

请完成以下笔试题，可以使用自己擅长的语言来编写，通过 github pull request 提交代码。

1. 编写一个递归版本的 reverse(s) 函数(或方法),以将字符串s倒置。
答案：
 public static String reverse(String str) {
        if (str.length() <= 1) {
            return str;
        }
        String substring = str.substring(1);
        char firstStr = str.charAt(0);
        return reverse(substring) + firstStr;
    }
2. 编写程序 expr，以计算从命令行输入的逆波兰表达式的值，其中每个运算符或操作数用一个单独的参数表示。例如，命令
expr 2 3 4 + *
答案：
public class Expr {
    public static void main(String[] args){

        String expression = "2 3 4 + *";
        //为了方便的取数据，我们将其放入到arraylist中
        List<String> list = getListString(expression);
        //遍历arraylist取到值完成逆波兰表达式的计算
        System.out.println(list);
        int res = calculate(list);
        System.out.println(res);
    }
    public static List<String> getListString(String exp){
        //将闯进来的exp分割
        String[] split = exp.split(" ");
        ArrayList<String> list = new ArrayList<String>();
        for (String item:split){
            list.add(item);
        }
        return list;
    }
    public static int calculate(List<String> list){
        //创建一个栈
        Stack<String> stack = new Stack<String>();
        //遍历list
        for (String item:list){
            //这里考虑如何判断是运算符还是数字，故这里使用正则表达式去求值
            if (item.matches("\\d+")){    //这里匹配的是多位数
                //数字直接入栈
                System.out.println(item);
                stack.push(item);
            }else{
                //说明是个符号
                //这里要将栈顶和次栈顶的元素出栈
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = 0;
                if (item.equals("+")){
                    res = num1 + num2;
                }else if (item.equals("-")){
                    res = num1 - num2;
                }else if (item.equals("*")){
                    res = num1 * num2;
                }else if (item.equals("/")){
                    res = num1 / num2;
                }else{
                    //System.out.println("没有该运算符~~~");
                    throw new RuntimeException("没有该运算符~~~");
                }
                //将res入栈
                stack.push(""+res);
            }
        }
        return Integer.parseInt(stack.pop());
    }
}

3. 用归并排序将3，1，4，1，5，9，2，6 排序。
答案：
public class MergeSort {

    public static void main(String[] args) {
        int[] a = {3,1,4,1,5,9,2,6};
        sort(a, 0, a.length -1);
        Arrays.stream(a).forEach(System.out::println);
    }


    public static int[] sort(int[] a, int low, int high) {
        int mid = (low + high) / 2;
        if (low < high) {
            //左边归并排序
            sort(a,low,mid);
            //右边归并排序
            sort(a,mid+1,high);
            //合并两个有序数组
            merge(a,low,mid,high);
        }
        return a;
    }

    public static void merge(int[] a, int low, int mid, int high) {
        int[] temp = new int[high - low + 1];
        int i = low;
        int j = mid + 1;
        int k = 0;
        while (i <= mid && j <= high) {
            // 对比大小，调整顺序
            if (a[i] < a[j]) {
                temp[k++] = a[i++];
            } else {
                temp[k++] = a[j++];
            }
        }
        // 右边剩余元素填充进temp中（因为前面已经归并，剩余的元素必会小于右边剩余的元素）
        while (i <= mid) {
            temp[k++] = a[i++];
        }
        // 右边剩余元素填充进temp中（因为前面已经归并，剩余的元素必会大于于左边剩余的元素）
        while (j <= high) {
            temp[k++] = a[j++];
        }
        // 调整数组顺序
        for (int x = 0; x < temp.length; x++) {
            a[x + low] = temp[x];
        }
    }
}
4. 对下面的 json 字符串 serial 相同的进行去重。

```javascript
  [{
    "name": "张三",
    "serial": "0001"
  }, {
    "name": "李四",
    "serial": "0002"
  }, {
    "name": "王五",
    "serial": "0003"
  }, {
    "name": "王五2",
    "serial": "0003"
  }, {
    "name": "赵四",
    "serial": "0004"
  }, {
    "name": "小明",
    "serial": "005"
  }, {
    "name": "小张",
    "serial": "006"
  }, {
    "name": "小李",
    "serial": "006"
  }, {
    "name": "小李2",
    "serial": "006"
  }, {
    "name": "赵四2",
    "serial": "0004"
  }];
```
答案：
<script>
   const data =[{
    "name": "张三",
    "serial": "0001"
  }, {
    "name": "李四",
    "serial": "0002"
  }, {
    "name": "王五",
    "serial": "0003"
  }, {
    "name": "王五2",
    "serial": "0003"
  }, {
    "name": "赵四",
    "serial": "0004"
  }, {
    "name": "小明",
    "serial": "005"
  }, {
    "name": "小张",
    "serial": "006"
  }, {
    "name": "小李",
    "serial": "006"
  }, {
    "name": "小李2",
    "serial": "006"
  }, {
    "name": "赵四2",
    "serial": "0004"
  }];
  const ChangeArr = (data) => {
    const newobj = {}
    if (data && data.length > 0) {
        data.forEach((item) => {
            newobj[item.serial] = item
        })
    }
    return newobj
}
console.log(ChangeArr(data), 'ChangeArr')
</script>

5. 把下面给出的扁平化json数据用递归的方式改写成组织树的形式

```javascript
  [
    {
      "id": "1",
      "name": "中国",
      "code": "110",
      "parent": ""
    },
    {
      "id": "2",
      "name": "北京市",
      "code": "110000",
      "parent": "110"
    },
    {
      "id": "3",
      "name": "河北省",
      "code": "130000",
      "parent": "110"
    },
    {
      "id": "4",
      "name": "四川省",
      "code": "510000",
      "parent": "110"
    },
    {
      "id": "5",
      "name": "石家庄市",
      "code": "130001",
      "parent": "130000"
    },
    {
      "id": "6",
      "name": "唐山市",
      "code": "130002",
      "parent": "130000"
    },
    {
      "id": "7",
      "name": "邢台市",
      "code": "130003",
      "parent": "130000"
    },
    {
      "id": "8",
      "name": "成都市",
      "code": "510001",
      "parent": "510000"
    },
    {
      "id": "9",
      "name": "简阳市",
      "code": "510002",
      "parent": "510000"
    },
    {
      "id": "10",
      "name": "武侯区",
      "code": "51000101",
      "parent": "510001"
    },
    {
      "id": "11",
      "name": "金牛区",
      "code": "51000102",
      "parent": "510001"
    }
  ];
```
答案：
<script>
    const flatArr = [
        {
            "id": "1",
            "name": "中国",
            "code": "110",
            "parent": ""
        },
        {
            "id": "2",
            "name": "北京市",
            "code": "110000",
            "parent": "110"
        },
        {
            "id": "3",
            "name": "河北省",
            "code": "130000",
            "parent": "110"
        },
        {
            "id": "4",
            "name": "四川省",
            "code": "510000",
            "parent": "110"
        },
        {
            "id": "5",
            "name": "石家庄市",
            "code": "130001",
            "parent": "130000"
        },
        {
            "id": "6",
            "name": "唐山市",
            "code": "130002",
            "parent": "130000"
        },
        {
            "id": "7",
            "name": "邢台市",
            "code": "130003",
            "parent": "130000"
        },
        {
            "id": "8",
            "name": "成都市",
            "code": "510001",
            "parent": "510000"
        },
        {
            "id": "9",
            "name": "简阳市",
            "code": "510002",
            "parent": "510000"
        },
        {
            "id": "10",
            "name": "武侯区",
            "code": "51000101",
            "parent": "510001"
        },
        {
            "id": "11",
            "name": "金牛区",
            "code": "51000102",
            "parent": "510001"
        }
    ];

    function getTreeData(arr, parent) {
        function loop(parent) {
            return arr.reduce((pre, cur) => {
                if (cur.parent === parent) {
                    cur.children = loop(cur.code)
                    pre.push(cur)
                }

                return pre
            }, [])
        }
        return loop(parent)
    }

    const result = getTreeData(flatArr, "")
    console.log('result', result)
</script>
