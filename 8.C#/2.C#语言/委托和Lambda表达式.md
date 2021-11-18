## 委托和Lambda表达式

> *委托*类型表示对具有**特定参数列表**和**返回类型**的方法的**引用**。 通过**委托**，可以将**方法**视为可分配给**变量**并可作为**参数**传递的实体。 *委托*还类似于其他一些语言中存在的**“函数指针”**概念。 与函数指针不同，委托是面向对象且类型安全的。

```csharp
using System;

namespace DelegateType
{
    /* 定义一个委托 */
    delegate double Function(double x);

    class Multiplier
    {
        double _factor; /* 私有成员变量 */
        public Multiplier(double factor) => _factor = factor;
        public double Multiply(double x) => x * _factor;
    }

    class Program
    {
        /* 使用委托的函数，f 是委托参数 */
        static double[] Apply(double[] a, Function f)
        {
            var result = new double[a.Length];
            for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
            return result;
        }

        static void Main(string[] args)
        {
            double[] a = { 0.0, 0.5, 1.0 };
            double[] squares = Apply(a, (x) => x * x);
            double[] sines = Apply(a, Math.Sin);
            Multiplier m = new Multiplier(2.0);
            double[] doubles = Apply(a, m.Multiply);

            foreach(var d in doubles)
            {
                Console.WriteLine($"{d}");
            }
            Console.WriteLine("按任意按钮退出程序!");
            Console.ReadKey();
        }
    }
}

```

输出结果：

```shell
0
1
2
按任意按钮退出程序!
```

> 委托不知道或不在意其所引用的方法的类。 引用的方法必须具有与委托相同的参数和返回类型。

