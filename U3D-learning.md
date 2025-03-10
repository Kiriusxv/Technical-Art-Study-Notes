# Unity学习笔记    
### 脚本的生命周期  
主要的两个是方法是start和update，start在第一帧开始前执行，而update则是每一帧执行一次。  
![其他生命周期方法](pic/U1.png)  
脚本的执行顺序除了在生命周期中控制以外，还能够直接在设置中直接进行管理  
![脚本执行顺序](pic/U2.png)
### 预制体与变体  
### 欧拉角与四元数 
### Debug功能
#### Debug.DrawLine与Debug.DrawRay  
```C#
Debug.DrawRay  
 public static void DrawRay (Vector3 start, Vector3 dir, Color color= Color.white, float duration= 0.0f,bool depthTest= true);  

Debug.DrawLine  
public static void DrawLine(Vector3 start, Vector3 end, Color color = Color.white, float duration = 0.0f, bool depthTest = true);
```  
区别在于传递的参数中，ray传递的是起点与方向，而line传递的是起点与终点，虽然ray直译过来是射线，但是这里用向量理解会更好，因为在unity中画出来的线是有长度的而不是像数学概念中的射线一样无限延伸，其长度取决于传入dir（方向向量）的长度。  
>ps：画线可以用LineRenderer，或者直接GL画，或者更方便的可以Debug.DrawLine，甚至可以将物理射线画出Debug.DrawRay，但是Debug画出的线只能在调试模式看得到，编译成游戏后将不再出现。
### 动态修改游戏物体
### Applicatio类
### 游戏时间
### 场景类
### 异步加载场景并获取进度  
协程方法异步加载场景
```C#
IEnumerator loadScene(){
    operation = SceneManager.loadSceneAsync(1);
    //加载完场景不要自动跳转
    operation.allowSceneActivation = false;
    yield return operation;
}
```
加载的进度
```C#
//0~0.9
operation.progress
```
>ps：0.9到完成一步到位  
### Transform的使用  
