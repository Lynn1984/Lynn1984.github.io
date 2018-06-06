---
title:  C#自定义消息处理
date: 2018-06-01 14:45:25
tags:
---

标签（空格分隔）： C#

---

## 通过SendMessage或postmessage函数发送

### 消息定义

```
public const int USER = 0x0400;
public const int WM_TEST =USER+101;
```

### 消息发送

```
[DllImport("User32.dll",EntryPoint="SendMessage")]
private static extern int SendMessage(
  IntPtr hWnd,      // 窗体句柄
  uint Msg,         // 消息的标识符
  uint wParam,      // 具体取决于消息
  uint lParam       // 具体取决于消息
  );

[DllImport("User32.dll",EntryPoint="PostMessage")]
private static extern int PostMessage(
  IntPtr hWnd,      // 接收消息的那个窗口的句柄。如设为HWND_BROADCAST，表示投递给系统中的所有顶级窗口。如设为零，表示投递一条线程消息（可参考PostThreadMessage）
  uint Msg,         // 消息的标识符
  uint wParam,      // 具体取决于消息
  uint lParam       // 具体取决于消息
);
```

### 消息接收
Form中是重载DefWinproc函数来处理接收的信息。

```
protected override void DefWndProc(ref Message m)
{
    switch(m.Msg)
    {
        case Message.WM_TEST:   //处理消息
        break;
        default:
        base.DefWndProc(ref m);   //调用基类函数处理非自定义消息。
        break;
    }
}
```

## 使用PostThreadMessage函数向线程发送消息

### 映射消息结构体原型和自定义消息

```
public struct tagMSG
{
    public int hwnd;
    public uint message;
    public int wParam;
    public long lParam;
    public uint time;
    public int pt;
}
public const int WM_CX_NULL = 0x400 + 100;
```

### 发送消息

```
[DllImport("user32.dll")]
private static extern int PostThreadMessage(
    int threadId,   //线程标识
    uint msg,      //消息标识符
    int wParam,   //具体由消息决定
    int lParam);  //具体由消息决定
```

Win32 API无法识别管理线程，你必须发送消息到Windows的线程ID上，而不是管理线程的ID上。

```
[DllImport("kernel32.dll")]
private static extern int GetCurrentThreadId();
private int _NewThreadId = GetCurrentThreadId();
PostThreadMessage(_NewThreadId, WM_CX_NULL, 1, 1);
```

### 接收消息

```
[DllImport("user32.dll")]
private static extern int GetMessage(
    ref tagMSG lpMsg, //指向MSG结构的指针，该结构从线程的消息队列里接收消息信息;
    int hwnd,//取得其消息的窗口的句柄。这是一个有特殊含义的值（NULL）。GetMessage为任何属于调用线程的窗口检索消息;
    int wMsgFilterMin, //指定被检索的最小消息值的整数
    int wMsgFilterMax); //指定被检索的最大消息值的整数
```