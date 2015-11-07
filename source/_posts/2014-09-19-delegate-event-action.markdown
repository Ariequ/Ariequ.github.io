---
layout: post
title: "Delegates Events and Actions in C#"
date: 2014-09-19 20:01:41 +0800
comments: true
categories: [Unity, CSharpLanguage]
---

* delegate  example  

使用delegate的目的是在方法中传递方法指针。通过声明delegate的方法签名，可以在运行时在符合delegate方法签名的方法中选择要执行的方法。  

``` C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace ModernLanguageConstructs
{
    class Program
    {
        // Part 1 - Explicit declaration of a delegate 
        //(helps a compiler ensure type safety)
        public delegate double delegateConvertTemperature(double sourceTemp);

        // A sample class to play with
        class TemperatureConverterImp
        {
            // Part 2 - Will be attached to a delegate later in the code
            public double ConvertToFahrenheit(double celsius)
            {
                return (celsius * 9.0/5.0) + 32.0;
            }

            //  Part 3 - Will be attached to a delegate later in the code
            public double ConvertToCelsius(double fahrenheit)
            {
                return (fahrenheit - 32.0) * 5.0 / 9.0;
            }
        }


        static void Main(string[] args)
        {
            //  Part 4 - Instantiate the main object
            TemperatureConverterImp obj = new TemperatureConverterImp();

            //  Part 5 - Intantiate delegate #1
            delegateConvertTemperature delConvertToFahrenheit =
                         new delegateConvertTemperature(obj.ConvertToFahrenheit);

            //  Part 6 - Intantiate delegate #2
            delegateConvertTemperature delConvertToCelsius =
                         new delegateConvertTemperature(obj.ConvertToCelsius);

            // Use delegates to accomplish work

            //  Part 7 - delegate #1
            double celsius = 0.0;
            double fahrenheit = delConvertToFahrenheit(celsius);
            string msg1 = string.Format("Celsius = {0}, Fahrenheit = {1}",
                                         celsius, fahrenheit);
            Console.WriteLine(msg1);

            //  Part 8 - delegate #2
            fahrenheit = 212.0;
            celsius = delConvertToCelsius(fahrenheit);
            string msg2 = string.Format("Celsius = {0}, Fahrenheit = {1}",
                                         celsius, fahrenheit);
            Console.WriteLine(msg2);
        }
    }
}
```
{% img /images/delegate_example_result.png %}  

* Event example  
Event是一种特殊类型的delegate，使用Event可以很方便的实现观察者模式。    

``` C#
    public class SampleEventArgs
    {
        public SampleEventArgs(string s) { Text = s; }
        public String Text {get; private set;} // readonly
    }
    public class Publisher
    {
        // Declare the delegate (if using non-generic pattern).
        public delegate void SampleEventHandler(object sender, SampleEventArgs e);

        // Declare the event.
        public event SampleEventHandler SampleEvent;

        // Wrap the event in a protected virtual method
        // to enable derived classes to raise the event.
        protected virtual void RaiseSampleEvent()
        {
            // Raise the event by using the () operator.
            if (SampleEvent != null)
                SampleEvent(this, new SampleEventArgs("Hello"));
        }
    }
```

* Action example  
Action是一种语法糖，它是一种不需要提前声明的delegate，使得delegate的使用更加方便，局限性就是不能定义返回值。（Func<> Delegates是一种可以定义返回值的语法糖）。  

``` C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace ModernLanguageConstructs
{
    class Program
    {
        static void Main(string[] args)
        {
            // Part 1 - First action that takes an int and converts it to hex
            Action<int> displayHex = delegate(int intValue)
            {
                Console.WriteLine(intValue.ToString("X"));
            };

            // Part 2 - Second action that takes a hex string and 
            // converts it to an int
            Action<string> displayInteger = delegate(string hexValue)
            {
                Console.WriteLine(int.Parse(hexValue,
                    System.Globalization.NumberStyles.HexNumber));
            };
            
            // Part 3 - exercise Action methods
            displayHex(16);
            displayInteger("10");
        }
    }
}
```

>参考资料：  
1. [C# Delegates, Actions, Funcs, Lambdas–Keeping it super simple](http://blogs.msdn.com/b/brunoterkaly/archive/2012/03/02/c-delegates-actions-lambdas-keeping-it-super-simple.aspx)  
2. [event（C# 参考）](http://msdn.microsoft.com/zh-cn/library/8627sbea.aspx)