### 数组

注意：在算法题中，并不知道它内部元素的情况。推荐使用构造函数创建数组的方法

> const arr = new Array();
> const arr = new Array(7); // 创建的是指定长度的空数组
> const arr = new Array(7).fill(1); // 创建的是长度为 7，且每个元素都初始化为 1 的数组

**遍历数组的方法**
性能：for > forEach > map(会返回一个全新的数组)

```js
arr.forEach((item, index) => {
  // 输出数组的元素值，输出当前索引
  console.log(item, index);
});
const newArr = arr.map((item, index) => {
  // arr 是没有任何变化的
  // 输出数组的元素值，输出当前索引
  console.log(item, index);
  // 在当前元素值的基础上加1
  return item + 1;
});
```

如果没有特殊的需要，那么统一使用 for 循环来实现遍历。从性能上看，for 循环遍历起来是最快的

### 二维数组(矩阵)

**二维数组的初始化：**

> 坑：const arr = (new Array(7)).fill([]);
> 这样会出错，因为这个入参的类型是引用类型，那么 fill 在填充坑位时填充的其实就是入参的引用

正规初始化一个二维数组：

```js
const len = arr.length;
for (let i = 0; i < len; i++) {
  // 将数组的每一个坑位初始化为数组
  arr[i] = [];
}
```

**二维数组的访问：**
需要两层循环

```js
// 缓存外部数组的长度
const outerLen = arr.length;
for (let i = 0; i < outerLen; i++) {
  // 缓存外部数组的长度
  const innerLen = arr[i].length;
  for (let j = 0; j < innerLen; j++) {
    // 输出数组的值，输出数组的索引
    console.log(arr[i][j], i, j);
  }
}
```

**将一个数组旋转 k 步**

- 输入一个数组[1, 2, 3, 4, 5, 6, 7]
- k = 3, 即旋转 3 步
- 输出[5, 6, 7, 1, 2, 3, 4]

- 思路 1：把末尾的元素挨个 pop，然后 unshift 到数组前面
- 思路 2：把数组拆分，最后 concat 拼接到一起 ---> 采用这种方式就很好

```js
function rotate(arr, k) {
  const length = arr.length;
  if (!k || length === 0) return arr;
  const step = Math.abs(k % length);

  const part1 = arr.slice(-step);
  const part2 = arr.slice(0, length - step);
  const part3 = part1.concat(part2);
  return part3;
}
```

### 栈和队列

在 js 中，栈和队列的实现一般都要依赖于数组

**在数组中增加元素的三种方法：**

- unshift 方法 - 添加元素到数组的头部 ---> 但是这种性能消耗是比较大的

```js
const arr = [1, 2];
arr.unshift(0); // [0, 1, 2]
```

- push 方法 - 添加元素到数组的尾部

```js
const arr = [1, 2];
arr.push(3); // [1, 2, 3]
```

- splice 方法 - 添加元素到数组的任何位置

```js
const arr = [1, 2];
arr.splice(1, 0, 3); // 在下标1的元素前面删除0个元素之后添加数字3  [1, 3, 2]
```

**在数组中删除元素的三种方法**

- shift 方法 - 删除数组头部的元素

```js
const arr = [1, 2, 3];
arr.shift();
```

- pop 方法 - 删除数组尾部的元素

```js
const arr = [1, 2, 3];
arr.pop();
```

- splice 方法 - 删除数组任意位置的元素

**栈，只用 pop 和 push 完成增删的“数组”**
**队列，只用 push 和 shift 完成增删的“数组”**

### 链表

由结点连接而成，在链表中，每个结点的结构都包括了两部分的内容：数据域 和 指针域
数据域存储的是当前结点所存储的数据值，而指针域则代表下一个结点(后继结点)的引用

```js
{
  // 数据域
  val: 1,
  // 指针域
  next: {
    val: 2,
    next: ...
  }
}
```

**链表结点的创建**

```js
function ListNode(val) {
  this.val = val;
  this.next = null;
}
const node = new ListNode(1);
node.next = new ListNode(2);
```

**链表元素的添加**

```js
// 要把node3 插入进node1 和 node2 之间
const node3 = new ListNode(3);
node3.next = node1.next; // 地址赋值
node1.next = node3; // 更改node1.next的引用地址
```

**链表元素的删除**

删除链表结点的操作，重点不是定位目标结点，而是定位目标结点的前驱结点

```js
// 把node3删掉：原本的链式结构： node1 -> node3 -> node2  更改为  node1 -> node2
node1.next = node3.next;
```

**链表元素的遍历**

```js
// 记录目标结点的位置
const index = 10;
// 设一个游标指向链表第一个结点，从第一个结点开始遍历
let node = head;
// 反复遍历到第10个结点为止
for (let i = 0; i < index && node; i++) {
  node = node.next; // node 仅仅是一个游标
}
```

**注意：js 中的数组有点特殊**

- 如果数组中只定义了一种类型的元素： const arr = [1, 2, 3, 4]
  - 它是一个纯数字数组，那么对应的确实是**连续内存**，但如果我们定义了不同类型的元素
  - > const arr = ['haha', 1, {a:1}]
- 它对应的就是一段非连续的内存。此时，JS 数组不再具有数组的特征，其底层使用哈希映射分配内存空间，是由对象链表来实现的

谨记“JS 数组未必是真正的数组”。所谓的真正数组，即“存储在连续的内存空间里”
**相较于数组，链表的一个优势是：添加和删除元素都不需要挪动多余的元素**
链表增删的复杂度是: O(1)
**相较于数组，链表的一个劣势是：读取某一个特定的链表结点时，必须遍历整个链表来查找它**
链表查找元素的复杂度: O(n), 数组可以按照索引一步到位，复杂度为: O(1)

### 树&二叉树

树：

- 树的层次计算规则： 根结点所在的那一层记为第一层，其子节点所在的就是第二层，以此类推...
- 结点和树的“高度”计算规则：叶子结点高度记为 1，每向上一层高度就加 1，逐层向上累加至目标结点时，所得到的的值就是目标结点的高度。树中结点的最大高度，称为“树的高度”。
- “度”的概念：一个结点开叉出去多少个子树，被记为结点的“度”
- “叶子结点”: 叶子结点就是度为 0 的结点

二叉树：

- 它可以没有根结点，作为一颗空树存在
- 如果它不是空树，那么必须由根结点、左子树和右子树组成，且左右子树都是二叉树(左右子树是不能交换的)

JS 中，二叉树使用对象来定义，结构分为三块：

- 数据域
- 左侧子结点(左子树根结点)的引用
- 右侧子结点(右子树根结点)的引用

```
// 二叉树结点的构造函数
function TreeNode(val) {
  this.val = val;
  this.left = this.right = null;   // 这里有点意思
}

const node = new TreeNode(1)
```

**二叉树的遍历** (可见有数组遍历、链表遍历、二叉树遍历)

- 先序遍历
- 中序遍历
- 后序遍历
- 层次遍历
  递归遍历(先、中、后序遍历) 迭代遍历(层次遍历)

#### 递归遍历

简单来说，当一个函数反复调用它自己的时候，递归就发生了

先序、中序、后序其实指的就是根结点的遍历时机

**编写递归函数的要点**

- 递归式：指的是重复的内容
- 递归边界：指的是什么时候停下来

```js
function preorder(root) {
  // 递归边界
  // 递归式
}
```

**前序遍历**

<!-- 打印结点 -->

```js
function preorder(root) {
  // 递归边界
  if (!root) {
    return;
  }

  console.log("当前遍历的结点是：", root.val);
  preorder(root.left);
  preorder(root.right);
}
```

**中序遍历**

<!-- 打印结点 -->

```js
function inorder(root) {
  // 递归边界
  if (!root) {
    return;
  }
  inorder(root.left);
  console.log("当前遍历的结点是：", root.val);
  inorder(root.right);
}
```

**后序遍历**

<!-- 打印结点 -->

```js
function postorder(root) {
  // 递归边界
  if (!root) {
    return;
  }
  // 递归式
  postorder(root.left);
  postorder(root.right);
  console.log("当前遍历的结点是：", root.val);
}
```

### 时间复杂度从小到大的顺序排列

O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) < O(2^n)

### 数组系列问题

**两数求和**

> 真题描述： 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

<!-- 讲究的是map方法 key存值  value存下标 -->

```js
function findTwoNum(nums, target) {
  let len = nums.length;
  let map = new Map();

  for (let i = 0; i < len; i++) {
    // 将值作为键
    if (map.has(target - nums[i])) {
      return [map[target - nums[i]], i];
    } else {
      map.set(nums[i], i);
    }
  }
}
```

**合并两个有序数组**

关键字： 有序

> 真题描述：给你两个**有序整数数组** nums1 和 nums2，请你将 nums2 合并到 nums1 中，**使 nums1 成为一个有序数组**。
> **双指针**

```js
function addTwoArr(nums1, m, nums2, n) {
  // 双指针   i  j
  let i = m - 1;
  let j = n - 1;
  let k = m + n - 1;

  while (i > 0 && j > 0) {
    // 逆序排列的
    if (nums1[i] > nums2[j]) {
      nums1[k] = nums1[i];
      k--;
      i--;
    } else {
      nums1[k] = nums2[j];
      k--;
      j--;
    }
  }
  while (j > 0) {
    nums1[k] = nums2[j];
    k--;
    j--;
  }
  return nums1;
}
```

**三数求和问题**

> 真题描述：给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且**不重复**的三元组。
> 这个会循环多次...

先要排成有序数组 然后是对撞指针 此题还得细看看

```js
function findThreeNum(nums) {
  let res = [];
  let len = nums.length;
  // 双指针法(对撞指针)，前提：数组有序
  nums = nums.sort((a, b) => a - b); // 这里是有返回值的
  for (let i = 0; i < len - 2; i++) {
    // 左指针 j
    let j = i + 1;
    // 右指针 k
    let k = len - 1;
    // 如果遇见重复的数字就跳过
    if (i > 0 && nums[i] === nums[i - 1]) {
      continue;
    }

    while (j < k) {
      // 三数之后小于0，左指针移动
      if (nums[i] + nums[j] + nums[k] < 0) {
        j++;
        // 解决重复值问题
        while (j < k && nums[j] === nums[j - 1]) {
          j++;
        }
        // 三数之和大于0，右指针移动
      } else if (nums[i] + nums[j] + nums[k] > 0) {
        k--;
        while (j < k && nums[k] === nums[k + 1]) {
          k--;
        }
        // 三数之和等于0，左右指针都要移动
      } else {
        res.push([nums[i], nums[j], nums[k]]);

        // 左右指针
        j++;
        k--;

        // 解决重复值问题
        while (j < k && nums[j] === nums[j - 1]) {
          j++;
        }
        while (j < k && nums[k] === nums[k + 1]) {
          k--;
        }
      }
    }
  }
  return res;
}
```

> 当看见关键词“有序”和“数组”，立即把双指针法想到，普通双指针走不通，就想到对撞指针

### 字符串系列问题

**反转字符串**

```js
const str = "abcdefg";
let res = str.split("").reverse().join("");
console.log(res);
```

**判断一个字符串是否是回文字符串**
(利用反转字符串的特性)

```js
function isPalindrome(str) {
  let res = str.split("").reverse().join("");
  return res === str;
}
```

(利用对称性来解决)

```js
function isPalindrome(str) {
  let len = str.length; // 字符串也有这个length属性，且没有括号
  for (let i = 0; i < len / 2; i++) {
    if (str[i] !== str[len - i - 1]) {
      return false;
    }
  }
  return true;
}
```

**回文字符串的衍生问题**

> 真题描述：给定一个非空字符串 s，最多删除一个字符。判断是否能成为回文字符串。
> (利用回文字符串的对称性来解决问题)

如果字符串中有“回文”这个关键字，就应该想到 **“对称性”** 和 **“双指针”** 来解决问题

```js
const validPalindrome = function (s) {
  // 缓存字符串长度
  let len = s.length;
  // 双指针
  let i = 0;
  let j = len - 1;

  while (i < j && s[i] === s[j]) {
    i++;
    j--;
  }

  if (isPalindrome(i + 1, j)) {
    return true;
  }
  if (isPalindrome(i, j - 1)) {
    return true;
  }

  function isPalindrome(start, end) {
    while (start < end) {
      if (s[start] < s[end]) return false;
      start++;
      end--;
    }
    return true;
  }
  // 默认返回 false
  return false;
};
```

**字符串匹配问题**

...

### 链表

**链表的合并**

> 真题描述：将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有结点组成的。

难度不大，就是穿针引线的过程

```js
const mergeTwoLists = function (l1, l2) {
  let head = new ListNode();
  let cur = head;
  while (l1 && l2) {
    if (l1.val <= l2.val) {
      cur.next = l1;
      l1 = l1.next;
    } else {
      cur.next = l2;
      l2 = l2.next;
    }
    cur = cur.next;
  }

  cur.next = l1 !== null ? l1 : l2;
  return head.next; // 这里只返回head.next 用得还挺巧妙的...
};
```

**链表的删除**

> 真题描述：给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

一般链表的循环都是 while 循环

```js
const deleteDuplicates = function (head) {
  let cur = head;
  // 不要写成while(cur)这样的格式，不然 0 '' 这种也会阻止循环的
  while (cur !== null && cur.next != null) {
    // 这里的循环条件记一下,因为要考虑只有0个或1个节点的情况
    if (cur.val === cur.next.val) {
      cur.next = cur.next.next;
    } else {
      // 注意，这里是else 而不是 每次循环都使用cur = cur.next
      cur = cur.next;
    }
  }
  return head;
};
```

**链表的删除：升级版**

> 真题描述：给定一个排序链表，删除所有含有重复数字的结点，只保留原始链表中 没有重复出现的数字。

会删除**所有结点**的情况则是双循环

```js
const deleteDuplicates = function (head) {
  // 只有零个或1个结点，则不会重复，直接返回
  while (head === null || head.next === null) {
    return head;
  }
  const dummy = new ListNode(); // dummy 结点
  dummy.next = head; // dummy结点就不动了
  let cur = dummy;
  while (cur.next !== null && cur.next.next !== null) {
    if (cur.next.val === cur.next.next.val) {
      let val = cur.next.val; // 减少不必要的麻烦，所以从重复的最开始的那个位置开始进行循环
      while (cur.next.val === val) {
        // 有空的时候还可以多想想
        cur.next = cur.next.next;
      }
    } else {
      cur = cur.next;
    }
  }
  return dummy.next;
};
```

**快慢指针-删除链表倒数第 N 个结点**

> 真题描述：给定一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点

```js
const removeNthFromEnd = function (head, n) {
  const dummy = new ListNode();
  dummy.next = head;

  let slow = dummy;
  let fast = dummy;

  // 快指针先走
  while (n) {
    fast = fast.next;
    n--;
  }
  // 现在快慢指针一起走
  while (fast.next !== null) {
    fast = fast.next;
    slow = slow.next;
  }

  // 慢指针跨掉倒数第n个结点
  slow.next = slow.next.next;

  return dummy.next;
};
```

**多指针-链表的反转**

> 真题描述：定义一个函数，输入一个链表的头结点，反转该链表并输出反转后链表的头结点。

```js
const reverseList = function (head) {
  // const dummy = new ListNode();  此题不需要dummy结点
  // 如果没有结点或只有一个结点，不必反转
  if (head === null || head.next === null) {
    return head;
  }

  let right = head.next;
  let mid = head;
  let left = null;

  while (mid.next !== null) {
    mid.next = left;

    // 移动位置
    left = mid;
    mid = right;
    right = right.next;
  }
  // 收尾的结点不能指向null
  mid.next = left;
  return mid;
};
```

**局部反转一个链表**

> 真题描述：反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

```js
const reverseBetween = function (head, m, n) {
  // 就比上面那个多一步计数而已
  if (head === null || head.next === null) {
    return head;
  }

  let right = head.next;
  let mid = head;
  let left = null;
  let leftIndex = null; // 来一个标记位
  while (m) {
    // 这里表示这三个指针正常向右移动到局部反转的位置
    if (m === 1) leftIndex = left;
    left = mid;
    mid = right;
    right = right.next;
    m--;
    n--;
  }
  while (n > 0) {
    mid.next = left;
    // 移动位置
    left = mid;
    mid = right;
    right = right.next;
    n--;
  }
  leftIndex.next.next = mid; // 这里的细节还是要好好注意
  leftIndex.next = left;
  return head;
};
```

**环形链表基本问题--如何判断链表是否成环**

> 真题描述：给定一个链表，判断链表中是否有环

```js
const hasCycle = function (head) {
  while (head) {
    if (head.tag === true) {
      return true;
    } else {
      head.tag = true;
      head = head.next;
    }
  }
  return false;
};
```

**环形链表衍生问题--定位环的起点**

> 真题描述：给定一个链表，返回链表开始入环的第一个结点。 如果链表无环，则返回 null。

```js
const hasCycle = function (head) {
  while (head) {
    if (head.tag === true) {
      return head;
    } else {
      head.tag = true;
      head = head.next;
    }
  }
  return null;
};
```

### 栈与队列的题型

**有效括号**

> 题目描述：给定一个只包括 `'('，')'，'{'，'}'，'['，']'` 的字符串，判断字符串是否有效

关键字：栈、存放开口向右的、三个判断就可以解决

```js
const flag = {
  "(": ")",
  "{": "}",
  "[": "]",
};
const isValid = function (s) {
  let len = s.length;
  let stack = []; // 存放开口向右的
  for (let i = 0; i < len; i++) {
    if (s[i] === "{" || s[i] === "[" || s[i] === "(") {
      stack.push(s[i]);
    } else if (flag[stack[stack.length - 1]] === s[i]) {
      stack.pop();
    } else {
      return false; // 既不能加又不会减，则可以说明给的不是对称的
    }
  }

  return stack.length === 0;
};
```

**栈问题-每日温度问题**

> 题目描述: 根据每日气温列表，请重新生成一个列表，对应位置的输出是需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 0 来代替。

关键字：栈递减问题、最小栈

```js
const dailyTemperatures = function (T) {
  let stack = [];
  let len = T.length;
  let arr = new Array(len).fill(0);
  for (let i = 0; i < len; i++) {
    if (stack.length === 0 || T[stack[stack.length - 1]] > T[i]) {
      stack.push(i);
    } else {
      while (T[stack[stack.length - 1]] < T[i]) {
        let temp = stack.pop(); // 出栈的时候计算那天的结果
        arr[temp] = i - temp;
      }
      stack.push(i);
    }
  }
  return arr;
};
```

**栈的设计--"最小栈"问题**

> 题目描述：设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

(注意这里的 this 使用)

```js
// 注意，这里自己写原型链
const MinStack = function () {
  this.stack = []; // 构造函数这里怎么也得整一个this
  this.minStack = []; // 弄一个栈递减
};

MinStack.prototype.push = function (x) {
  this.stack.push(x);
  if (
    this.stack2.length === 0 ||
    x <= this.minStack[this.minStack.length - 1] // 包括等于
  ) {
    this.minStack.push(x);
  }
};

MinStack.prototype.pop = function () {
  if (
    this.stack[this.stack.length - 1] ===
    this.minStack[this.minStack.length - 1]
  ) {
    this.minStack.pop();
  }
  this.stack.pop();
};

MinStack.prototype.top = function () {
  return this.stack[this.stack.length - 1];
};

MinStack.prototype.getMin = function () {
  return this.minStack[this.minStack.length - 1];
};
```

**如何用栈实现一个队列**

> 题目描述： 使用栈实现队列的下列操作：push pop peek empty

```js
const MyQueue = function () {
  // 初始化两个栈
  this.stack1 = [];
  this.stack2 = [];
};

MyQueue.prototype.push = function (x) {
  // if () {
  this.stack1.push(x);
  // }
};

MyQueue.prototype.pop = function () {
  let len = this.stack1.length;
  for (let i = 0; i < len; i++) {
    this.stack2.push(this.stack1.pop());
  }
  let top = this.stack2.pop();
  let len2 = this.stack2.length;
  for (let i = 0; i < len2; i++) {
    this.stack1.push(this.stack2.pop());
  }
  return top;
};

MyQueue.prototype.peek = function () {
  let len = this.stack1.length;
  for (let i = 0; i < len; i++) {
    this.stack2.push(this.stack1.pop());
  }
  let top = this.stack2[this.stack2.length - 1];
  let len2 = this.stack2.length;
  for (let i = 0; i < len2; i++) {
    this.stack1.push(this.stack2.pop());
  }
  return top;
};

MyQueue.prototype.empty = function () {
  return this.stack1.length === 0;
};
```

**双端队列 -- 滑动窗口**
双端队列就是允许在队列的两端继续插入和删除的队列
双端队列法，核心思路是维护一个有效的递减队列 (有待再熟悉熟悉)

```js
const maxSlidingWindow = function (nums, k) {
  let queue = []; // 一个递减栈  --> 存放的元素是下标
  let len = nums.length;
  let arr = []; // 存放所有窗口的最大值
  for (let i = 0; i < len; i++) {
    if (queue[0] < i - k + 1) queue.shift(); // 这个要注意一下，出队时机
    if (queue.length === 0 || nums[queue[queue.length - 1]] >= nums[i]) {
      queue.push(i);
    } else {
      while (queue.length !== 0 && nums[queue[queue.length - 1]] < nums[i]) {
        queue.pop();
      }
      queue.push(i);
    }
    if (i >= k - 1) {
      arr.push(nums[queue[0]]);
    }
  }
  return arr;
};
```

### DFS 与 BFS

### 二叉树的遍历

- 前序遍历：root->left->right
- 中序遍历：left->root->right
- 后序遍历：left->right->root

这三种遍历用递归非常简单，仅仅把打印顺序换一下就可以

**DFS (deep) 深度优先遍历**
**BFS 广度优先遍历**
只要没碰壁，就决不选择其他的道路，而是坚持向当前道路的深处挖掘 --> 这就是所谓的深度优先搜索 (试图穷举完所有的完整路径)

深度优先搜索的本质 --- 栈结构：往往使用递归来模拟入栈和出栈的逻辑
递归式就是我们选择道路的过程，而递归边界就是死胡同

**广度优先搜索**
并不执着于”一往无前“，更关心的是眼下自己能够直接到达的所有目标，其动作有点类似”扫描“
“广度”为第一要务、雨露均沾，一层一层地扫描
广度优先 和 队列有着密不可分的关系

**BFS 实战: 二叉树的层序遍历**

```js
function BFS(root) {
  const queue = [];
  // 根结点先入队
  queue.push(root);
  // 队列不为空，说明没有遍历完全
  while (queue.length) {
    const top = queue[0];
    console.log(top.val);
    if (top.left) {
      queue.push(top.left);
    }
    if (top.right) {
      queue.push(top.right);
    }
    queue.shift();
  }
}
```

**BFS 实战：二叉树的锯齿形层序遍历**

未做完，以后再做这个

```js
function leftToRight(queue, top) {
    if (top.left) {
      queue.push(top.left);
    }
    if (top.right) {
      queue.push(top.right);
    }
}
function rightToLeft(queue, top) {
    if (top.right) {
      queue.push(top.right);
    }
    if (top.left) {
      queue.push(top.left);
    }
}
function zigzagLevelOrder = function(root) {
  const queue = [];
  const result = [];
  let count = 0;     // 计数器
  queue.push(root);
  while (queue.length) {
    let top = queue[0];
    count % 2 !== 0 ? leftToRight(queue, top) : rightToLeft(queue, top);
    queue.shift();
  }
  return result;
}
```

**全排列问题**

> 给定一个没有重复数字的序列，返回其所有可能的全排列

```js
// 入参是一个数组
const permute = function (nums) {
  // 缓存数组的长度
  const len = nums.length;
  const res = [];
  let curr = [];
  let map = new Map();
  // 内部造一个递归(深度优先遍历)
  function dfs(nth) {
    if (nth === len) {
      res.push(curr.slice()); // curr.slice() 浅拷贝一个数组
      return;
    }

    for (let i = 0; i < len; i++) {
      if (map.has(nums[i])) {
        continue;
      } else {
        map.set(nums[i], true);
        curr.push(nums[i]);
        dfs(nth + 1);
        map.delete(nums[i]);
        curr.pop();
      }
    }
  }
  dfs(0);
  return res;
};
```

**组合问题：变化的“坑位”，不变的“套路”**

> 题目描述：给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）
> 其实就是二叉树 **取 或 不取**

```js
const subsets = function (nums) {
  let res = [];
  let curr = [];
  let len = nums.length;

  // 还是 dfs 深度优先遍历
  function dfs(nth) {
    if (nth === len) {
      res.push(curr.slice());
      return;
    }
    for (let i = 0; i < len; i++) {
      // 不取
      dfs(nth + 1);
      // 取
      curr.push(nums[i]);
      dfs(nth + 1);
      curr.pop();
    }
  }

  dfs(0);
  return res;
};
```

**限定组合问题**

> 题目描述：给定两个整数 n 和 k，返回 1...n 中所有可能的 k 个数的组合。

```js

```

... 待做

**迭代法 先序遍历**
当一道题 明明可以用递归做出来，但是突然让你用迭代，此时，你要本能的往栈上想

```js
const preorderTraversal = function (root) {
  const res = [];
  // 处理边界条件
  if (!root) {
    return res;
  }

  // 初始化栈结构
  const stack = [];
  stack.push(root);

  while (stack.length) {
    let cur = stack.pop();
    res.push(cur.val);
    // 先放右节点入栈
    if (cur.right) {
      stack.push(cur.right);
    }
    if (cur.left) {
      stack.push(cur.left);
    }
  }

  return res;
};
```

**迭代法 后续遍历**
(好烦这题...)

```js
const tree = (root) => {
  let res = [];
  if (!root) {
    return res;
  }

  // 初始化栈结构
  const stack = [];
  stack.push(root);
  // 左 -->  右  --> 根
  while (stack.length) {
    let cur = stack.pop();
    res.unshift(cur.val);
    if (cur.left) {
      stack.push(cur.left);
    }
    if (cur.right) {
      stack.push(cur.right);
    }
  }
  return res;
};
```

### 二叉搜索树

BST: 左子树上**所有节点**的数据域都小于等于根结点的数据域，右子树上**所有结点**的数据域都大于等于根结点的数据域

一颗子树应该满足：左孩子 <= 根结点 <= 右孩子

**查找二叉树中的某一结点的值**

```js
const search(root, n) {
  if (!root) {
    return;
  }

  if (root.val === n) {
    return root;
  } else if (root.val < n){
    search(root.right, n);
  } else if (root.val > n) {
    search(root.left, n);
  }
}
```

**插入结点**
(前提，所有结点都是独一无二的)

```js
const add(root, n) {
  if (!root) {
    root = new ListNode(n);
    return root;
  }
  if (root.val > n) {
    root.left = insertIntoBST(root.left, n);
  } else {
    // 当前节点数据与小于n，向右查找
    root.right = insertIntoBST(root.right, n);
  }
  return root;
}
```

**删除结点**

```js
function deleteNode(root, n) {
  if (!root) {
    return root;
  }
  if (root.val === n) {
    if (!root.left && !root.right) {
      root = null;
    }
  } else if (root.val > n) {
    root.left = deleteNode(root.left, n);
  } else {
    root.right = deleteNode(root.right, n);
  }
  return root;
}
```

**注意：二叉搜索树的中序遍历序列是有序的**

```js
const sortedArrayToBST = function (nums) {
  // 处理边界条件
  if (!nums.length) {
    return null;
  }

  // root 结点是递归“提”起数组的结果
  const root = buildBST(0, nums.length - 1);

  // 定义二叉树构建函数，入参是子序列的索引范围
  function buildBST(low, high) {
    // 当 low > high 时，意味着当前范围的数字已经被递归处理完全了
    if (low > high) {
      return null;
    }
    // 二分一下，取出当前子序列的中间元素
    const mid = Math.floor((high + low) / 2);
    // 将中间元素的值作为当前子树的根结点值
    const cur = new TreeNode(nums[mid]);
    // 递归构建左子树，范围二分为[low,mid)
    cur.left = buildBST(low, mid - 1);
    // 递归构建左子树，范围二分为为(mid,high]
    cur.right = buildBST(mid + 1, high);
    // 返回当前结点
    return cur;
  }
  // 返回根结点
  return root;
};
```

**求一个二叉搜索树的第 K 小值**
用二分法来查找 时间复杂度 O(logn)
二叉搜索树的中序遍历 就是从小到大顺序排列的

```js
let arr = [];
function inorderTree(root) {
  if (!root) return null;
  inorderTree(root.left);
  arr.push(root.val);
  inorderTree(root.right);
}
function test(root, k) {
  inorderTree(root);
  return arr[k - 1];
}
```

### 排序

**分治，分而治之**其思想就是将一个大问题分解为若干子问题，针对子问题分别求解后，再将子问题的解整合为大问题的解

- 分解子问题
- 求解子问题
- 合并子问题的解，得出大问题的解

**归并排序**
归并排序在实现上依托的就是递归思想
(时间复杂度 O(nlog(n)))

```js
function mergeSort(arr) {
  const len = arr.length;
  // 处理边界情况
  if (let <= 1) {
    return arr;
  }
  // 计算分割点
  const mid = Math.floor(len / 2); // 表示如果是奇数，则前部分取小的
  const leftArr = mergeSort(arr.slice(0, mid)); // 不包含mid
  const rightArr = mergeSort(arr.slice(mid, len));
  arr = mergeArr(leftArr, rightArr);
  return arr;
}

function mergeArr(arr1, arr2) {
  let i = 0,
    j = 0;
  const res = [];
  const len1 = arr1.length;
  const len2 = arr2.length;
  while (i < len1 && j < len2) {
    if (arr1[i] < arr2[j]) {
      res.push(arr1[i]);
      i++;
    } else {
      res.push(arr2[j]);
      j++;
    }
  }
  if (i < len1) {
    return res.concat(arr1.slice(i));
  } else {
    return res.concat(arr2.slice(j));
  }
}
```

**快速排序**
快速排序在基本思想上和归并排序是一致的。只不过不会把数组分割再合并到新数组中，而是直接在原来的数组内部进行排序

- 固定算法，固定思路

  - 找到中间位置 midValue
  - 遍历数组，小于 midValue 放在 left，否则放在 right
  - 继续递归，最后 concat 拼接，返回
    (一次遍历 + 一次二分)

- 获取 midValue 的两种方式：
  - 使用 splice，会修改原数组(会修改数组，但是数组是连续的空间，所以会消耗更多的时间)
  - 使用 slice，不会修改原数组 --- 更加推荐

```js
function quickSort(arr, left = 0, right = arr.length - 1) {
  if (arr.length > 1) {
    // 划分左右数组的基准线
    const lineIndex = partition(arr, left, right);
    // 分解为子问题
    if (left < lineIndex - 1) {
      quickSort(arr, left, lineIndex - 1);
    }
    if (lineIndex < right) {
      quickSort(arr, lineIndex, right);
    }
  }
  return arr;
}

function partition(arr, left, right) {
  // 基准值默认取中间位置的元素
  let pivotValue = arr[Math.floor(left + (right - left) / 2)];
  // 初始化左右指针
  let i = left;
  let j = right;

  while (i <= j) {}
}

function swap(arr, i, j) {
  [arr[i], arr[j]] = [arr[j], arr[i]];
}
```

**用 JS 实现二分查找，并说明时间复杂度**
找中间位置...
每次都二分之一
二分是有神奇力量的...

- 递归-代码逻辑更加清晰
- 非递归-性能够好(所有的递归都可以改成非递归的形式)
- 时间复杂度 O(logn) 非常快

```js
function binarySearch1(arr, target) {
  const length = arr.length;
  if (length === 0) return -1;

  let startIndex = 0; // 开始位置
  let endIndex = length - 1; // 结束位置

  while (startIndex <= endIndex) {
    const midIndex = Math.floor((startIndex + endIndex) / 2);
    const midValue = arr[midIndex];
    if (target < midValue) {
      // 目标值较小，在左侧查找
      endIndex = midIndex - 1;
    } else if (target > midValue) {
      // 目标较大，则继续在右侧找
      startIndex = midIndex + 1;
    } else {
      return midIndex;
    }
  }
  return -1;
}
```

### 动态规划

**斐波那契数列**

```js
// 非常耗性能
// 递归 - 大量的重复计算   时间复杂度 O(2^n)  从后往前算
function finonacci(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;

  return finonacci(n - 1) + finonacci(n - 2);
}

// 时间复杂度的优化
// 不用递归，用循环记录中间结果  时间复杂度 O(n)

function finonacci(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;

  let n1 = 1; // 记录n - 1的结果
  let n2 = 0; // 记录n - 2的结果
  let res = 0;

  for (let i = 2; i <= n; i++) {
    res = n1 + n2;

    // 记录中间结果
    n2 = n1;
    n1 = res;
  }
  return res;
}
```

**爬楼梯**

```js
function climb(n) {
  let f = [];
  f[1] = 1;
  f[2] = 2;

  for (let i = 2; i <= n; i++) {
    f[i] = f[i - 1] + f[i - 2];
  }

  return f[n];
}
```

要求给出个数而不是具体的路径 动态规划来解决

思路：
先倒推： f(n) = f(n - 1) + f(n - 2);

```js
const climbStairs = function (n) {
  const f = [];
  f[1] = 1;
  f[2] = 2;
  for (let i = 3; i <= n; i++) {
    f[i] = f[i - 1] + f[i - 2];
  }
  return f[n];
};
```

动态规划：

- 把一个大问题，拆解为多个小问题，逐级向下拆解
- 用递归的思路去分析问题，再改为循环(一般都是 for 循环)来实现

**算法三大思维： 贪心、二分、动态规划**

**将数组中的 0 移动到末尾**

- 如输入[1, 0 , 3, 0, 11, 0] 输出[1, 3, 11, 0, 0, 0]
- 只移动 0，其他顺序不变
- 必须在原数组进行操作

- 传统思路： 遍历数组，遇到 0 则 push 到数组末尾
- 用 splice 截取掉当前元素
- 时间复杂度 O(n^2) 不可用 (双层循环 splice 算一层)
- 注意: 数组是连续存储空间，要慎用 splice，unshift 等 API

- 双指针法
- 有序

```js
function moveZero(arr) {
  if (arr.length === 0) return;

  let i;
  let j = -1; // 指向第一个 0

  for (i = 0; i < length; i++) {
    if (arr[i] === 0) {
      if (j < 0) {
        j = i;
      }
    }

    if (arr[i] !== 0 && j >= 0) {
      // 交换
      [arr[i], arr[j]] = [arr[j], arr[i]];
      j++;
    }
  }
}
```

### 字符串中连续最多的字符，以及次数

- 传统思路：嵌套循环 看似时间复杂度 O(n^2) 但实际是 O(n),因为有跳步(i 跳到 j 那里)

**分治**
这里主要是针对**排序算法**
分成子问题，解决子问题，合成大问题的解

核心思想是二分

时间复杂度 O(nlogn) ---> 因为每一层都是一次二分 一次遍历

```js
function mergeSort(arr) {
  const len = arr.length;
  if (len <= 1) return arr;

  // 二分
  const mid = Math.floor(len / 2);
  // 递归...
  const leftArr = mergeSort(arr.slice(0, mid));
  const rightArr = mergeSort(arr.slice(mid, len));

  arr = mergeArr(leftArr, rightArr);
  return arr;
}

// 合并两个有序的数组
function mergeArr(arr1, arr2) {
  let l1 = 0;
  let l2 = 0;
  let arr = [];
  let len1 = arr1.length;
  let len2 = arr2.length;
  while (l1 < len1 && l2 < len2) {
    if (arr1[l1] <= arr2[l2]) {
      arr.push(arr1[l1]);
      l1++;
    } else {
      arr.push(arr2[l2]);
      l2++;
    }
  }

  if (l1 < len1) {
    return arr.concat(arr1.slice(l1));
  } else {
    return arr.concat(arr2.slice(l2));
  }
}
```

**快排**
快排和归并思维类似，只不过，快排设立的中间值要求，小的就放在左边，大的就放在右边...

关键词： 基准 双指针

平均时间复杂度： O(nlog(n))  
最好时间复杂度： O(nlog(n))
最坏时间复杂度： O(n^2)

- 找到中间位置 midValue
- 遍历数组，小于 midValue 放在 left，否则放在 right
- 继续递归，最后 concat 拼接，返回
- 使用 splice，会修改元素组 --> 除非面试官说了要修改元素组，才用这个
- 使用 slice，不会修改原数组 --- 更加推荐

```js
// 使用splice
function quickSort(arr) {
  const length = arr.length;
  if (length === 0) return arr;

  const midIndex = Math.floor(length / 2);
  const midValue = arr.splice(midIndex, 1)[0];

  const left = [];
  const right = [];

  for (let i = 0; i < arr.length; i++) {
    const n = arr[i];
    if (n < midValue) {
      // 小于midValue, 则放在left
      left.push(n);
    } else {
      right.push(n);
    }
  }
  // 递归...
  return quickSort(left).concat([midValue], quickSort(right));
}
```

```js
function quickSort(arr) {
  const length = arr.length;
  if (length === 0) return arr;

  const midIndex = Math.floor(length / 2);
  const midValue = arr.slice(midIndex, midIndex + 1)[0];

  const left = [];
  const right = [];

  for (let i = 0; i < length; i++) {
    if (i !== midIndex) {
      const n = arr[i];
      if (n < midValue) {
        // 小于midValue, 则放在left
        left.push(n);
      } else {
        right.push(n);
      }
    }
  }

  return quickSort(left).concat([midValue], quickSort(right));
}
```

冒泡 选择 插入 都是双循环... 时间复杂度 O(n^2)
下面三个的要求，就是默写...
**冒泡排序**

```js
function bubble(arr) {
  const len = arr.length;
  for (let i = 0; i < len; i++) {
    for (let j = 0; j < len - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  return arr;
}
```

**选择排序**
找到最小值，交换

时间复杂度： O(n^2)

```js
function selectSort(arr) {
  const len = arr.length;

  let minIndex;
  for (let i = 0; i < len; i++) {
    minIndex = i;
    for (let j = i; j < len; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    if (minIndex !== i) {
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
    }
  }
  return arr;
}
```

**插入排序**
找到元素在它前面那个序列中的正确位置
(当前元素前面的序列都是有序的)

```js
function insertSort(arr) {
  const len = arr.length;

  let temp;
  for (let i = 1; i < len; i++) {
    let j = i;
    temp = arr[i];
    while (j > 0 && arr[j - 1] > temp) {
      // 这是在往前退
      arr[j] = arr[j - 1];
      j--;
    }
    arr[j] = temp;
  }
  return arr;
}
```

**最长公共前缀**

```js
var longestCommonPrefix = function(strs) {
  if (strs.length === 0) return '';

  let len = strs.length;
  let str = strs[0];
  for (let i = 0; i < len; i++) {
    let j = 0
    for (; j < str.length && strs[i].length; j++) {
      if (str[j] !== strs[i][j]) return break;
    }
    str = str.substr(0, j);
    if (ans === '') return ans;
  }
  return str;
}
```

**无重复字符的最长子串 力扣 3**
set 大法

```js
var lengthOfLongestSubstring = function (s) {
  const set = new Set();
  const len = s.length;

  for (let i = 0; i < len; i++) {
    while (!set.has(s[right]) && right < len) {
      set.add(s[right]);
      right++;
    }
    max = Math.max(max, set.size);
    set.delete(s[i]);
  }
  return max;
};
```

**最长回文子串 力扣 4**
给你一个字符串 s，找到 s 中最长的回文子串。
(动态规划)

```js

```

**迭代法实现 前中后序遍历**
先遍历 根结点 --> 左孩子 --> 右孩子
将递归 转换成 栈结构来解决问题

**前序**
while 循环 + 栈进出

```js
const preorder = function (root) {
  // 定义结果数组
  const res = [];
  if (!root) {
    return res;
  }
  // 初始化栈结构
  const stack = [];
  stack.push(root);
  while (stack.length) {
    const cur = stack.pop();
    res.push(cur.val);
    if (cur.right) {
      stack.push(cur.right);
    }
    if (cur.left) {
      stack.push(cur.left);
    }
  }
  return res;
};
```

**后序**

左 -> 右 -> 根

```js
const postorder = function (root) {
  // 定义结果数组
  const res = [];
  if (!root) {
    return res;
  }
  // 初始化栈结构
  const stack = [];
  stack.push(root);
  while (stack.length) {
    const cur = stack.pop();
    res.unshift(cur.value); // 这一点和前序不一样
    if (cur.left) {
      // 这一点的顺序有变化
      stack.push(cur.left);
    }
    if (cur.right) {
      stack.push(cur.right);
    }
  }
  return res;
};
```

**中序遍历**

左 -> 根 -> 右

```js
const midorder = function (root) {
  const res = [];
  const stack = [];
  let cur = root;

  while (cur || stack.length) {
    while (cur) {
      stack.push(cur); // 左节点全部放进去
      cur = cur.left;
    }
    // 取出栈顶元素
    cur = stack.pop();
    res.push(cur.val);
    cur = cur.right;
  }

  return res;
};
```

dfs 会遍历完所有的路线... 栈 ---> 一般用递归来模拟入栈和出栈

**层序遍历**

bfs 层序遍历 ---> 队列来解决

```js
function bfs(root) {
  const queue = [];
  queue.push(root);

  while (queue.length) {
    const top = queue.shift();
    console.log(top.val);
    if (top.left) {
      queue.push(top.left);
    }
    if (top.right) {
      queue.push(top.right);
    }
  }
}
```

```js
const levelOrder = function (root) {
  const res = [];
  if (!root) {
    return res;
  }
  // 初始化列表
  const queue = [];
  queue.push(root);
  while (queue.length) {
    const level = [];
    const len = queue.length;
    for (let i = 0; i < len; i++) {
      const top = queue.shift();
      level.push(top.val);
      if (top.left) {
        queue.push(top.left);
      }
      if (top.right) {
        queue.push(top.right);
      }
    }
    res.push(level);
  }
  return res;
};
```

**反转二叉树**
递归，交换

```js
const invertTree = function (root) {
  // 定义递归边界
  if (!root) {
    return root;
  }
  // 递归交换右孩子的子结点
  let right = invertTree(root.right); // left right 设置的顺序不一样
  // 递归交换左孩子的子结点
  let left = invertTree(root.left);
  // 交换当前遍历到的两个左右孩子结点
  root.left = right;
  root.right = left;
  return root;
};
```

**全排列问题**

穷举、递归

```js
const permute = function (nums) {
  // 缓存数组的长度
  const len = nums.length;
  const curr = [];
  const res = [];
  const visited = {};
  function dfs(nth) {
    if (nth === len) {
      res.push(curr.slice()); // 这里也是一个关键操作...
      return;
    }
    for (let i = 0; i < len; i++) {
      if (!visited[nums[i]]) {
        visited[nums[i]] = 1;
        curr.push(nums[i]);
        dfs(nth + 1);
        curr.pop();
        visited[nums[i]] = 0;
      }
    }
  }
  dfs(0);

  return res;
};
```

**斐波拉契数列非递归实现-->就是使用动态规划...和上面做的方式一样**

**自己实现一个五子棋判断输赢函数**
想法是，二维数组来决定你的落子点，map 来存储用户下过的位置，并做上标记。然后通过 for 循环注意遍历 8 个方向

**无重复字符的最长子串**

```js
// 滑动窗口
var lengthOfLongestSubstring = function (s) {
  let left = 0;
  let max = 0;
  let len = s.length;
  let map = new Map(); // 这里的map就设置的很好，key存值，value存下标
  for (let right = 0; right < len; right++) {
    if (map.has(s[right]) && map.get(s[right]) >= left) {
      left = map.get(s[right]) + 1;
      // map.delete(s[right])  这样只能删掉一个元素
    }
    max = Math.max(max, right - left + 1);
    map.set(s[right], right);
  }
  return max;
};
```

**合并二叉树**

```js
var mergeTrees = function (root1, root2) {
  if (!root1 && root2) {
    return root2;
  } else if (root1 && !root2) {
    return root1;
  }

  function dfs(root1, root2) {
    if (!root1 && !root2) return;

    if (root1 && root2) {
      root1.val = root1.val + root2.val;
    }

    // 关键点在于存在单个结点时要补充另一个结点
    if (!root1.left && root2.left) {
      root1.left = new TreeNode(0);
    } else if (root1.left && !root2.left) {
      root2.left = new TreeNode(0);
    }
    dfs(root1.left, root2.left);

    if (!root1.right && root2.right) {
      root1.right = new TreeNode(0);
    } else if (root1.right && !root2.right) {
      root2.right = new TreeNode(0);
    }
    dfs(root1.right, root2.right);
  }
  dfs(root1, root2);
  return root1;
};
```

**比较版本号**

```js
var compareVersion = function (version1, version2) {
  const arr1 = version1.split(".");
  const arr2 = version2.split(".");

  while (arr1.length && arr2.length) {
    const n1 = Number(arr1.shift()); // Number('000')  --> 0    Number('001')  --> 1
    const n2 = Number(arr2.shift());

    if (n1 > n2) return 1;
    if (n1 < n2) return -1;
  }
  if (arr1.length) {
    return arr1.every((item) => Number(item) === 0) ? 0 : 1;
  }
  if (arr2.length) {
    return arr2.every((item) => Number(item) === 0) ? 0 : -1;
  }
  return 0;
};
```

**相交链表**

```js
// 相交链表用set来解决
var getIntersectionNode = function (headA, headB) {
  let visited = new Set();

  while (headA) {
    visited.add(headA);
    headA = headA.next;
  }
  while (headB) {
    if (visited.has(headB)) return headB;
    headB = headB.next;
  }
  return null;
};
```

**合并区间**

```js
var merge = function (intervals) {
  // 先排序减少一些复杂问题
  intervals = intervals.sort((a, b) => a[0] - b[0]);
  let len = intervals.length;
  let res = [];
  let cur = [];
  for (let i = 0; i < len; i++) {
    if (cur.length === 0) {
      cur = intervals[i].slice();
    } else if (cur[1] >= intervals[i][0] && cur[1] < intervals[i][1]) {
      // 这里是重点
      cur[1] = intervals[i][1];
    }
    if (i === len - 1 || cur[1] < intervals[i + 1][0]) {
      res.push(cur);
      cur = [];
    }
  }
  return res;
};
```

**岛屿的最大面积**

```js
//  dfs(深度优先搜索)
var maxAreaOfIsland = function (grid) {
  let x = grid.length;
  let y = grid[0].length;
  let max = 0;

  for (let i = 0; i < x; i++) {
    for (let j = 0; j < y; j++) {
      if (grid[i][j] === 1) {
        max = Math.max(max, dfs(grid, i, j, x, y));
      }
    }
  }
  return max;
};

function dfs(grid, i, j, x, y) {
  if (i < 0 || i >= x || j < 0 || j >= y || grid[i][j] == 0) return 0;
  let count = 1;
  grid[i][j] = 0; // 下次就不能再计算这个小岛了
  // 四个方向递归
  count += dfs(grid, i + 1, j, x, y);
  count += dfs(grid, i, j + 1, x, y);
  count += dfs(grid, i - 1, j, x, y);
  count += dfs(grid, i, j - 1, x, y);
  return count;
}
```

**两数相加**

<!-- 思路，全把链条放在l1上面。如果l1短，就把l2多的部分复制给l1 -->

```js
var addTwoNumbers = function (l1, l2) {
  // 说明这个是会进1位的
  let flag = 0;
  let head = l1;
  let pre = null;
  while (l1 && l2) {
    l1.val += flag;
    flag = 0;
    l1.val = l1.val + l2.val;
    if (l1.val >= 10) {
      flag++;
      l1.val -= 10;
    }
    pre = l1; // 这里没有连接起来
    l1 = l1.next;
    l2 = l2.next;
  }
  if (!l1 && !l2 && flag) {
    l1 = pre;
    l1.next = new ListNode(flag);
    return head;
  }
  l1 = pre;
  // 这种情况就是只有l2.next 没有l1.next
  while (l2) {
    pre.next = new ListNode(l2.val);
    pre = pre.next;
    l2 = l2.next;
  }
  l1 = l1.next;
  while (l1) {
    l1.val += flag;
    flag = 0;
    if (l1.val >= 10) {
      flag++;
      l1.val -= 10;
    }
    if (flag && !l1.next) {
      l1.next = new ListNode(0);
    }
    l1 = l1.next;
  }
  // 全部放在l1链表上
  return head;
};
```

**最长公共子序列**

**不同路径**
