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
    //加载完场景不自动跳转
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
常用数据示例 
```C#
//获取位置
transform.position//世界位置 
transform.localPosition//相对位置，相对于父位置
//获取旋转
transform.rotation//四元数
transform.localRotation
transform.eulerAngles//欧拉角
transform.localEulerAngles
//获取缩放
transform.localScale
//向量
transform.forward
transform.right
transform.up
```
常用方法示例
```C#
//看向0点
transform.LookAt(Vector3.zero);
//旋转
transform.Rotate(Vector.up,1);
//绕某个物体旋转
transfoem.RotateAround(Vector3.zero,Vector.up,5);
//移动
transform.Translate(Vector.forward * 0.1f);
```
父子关系
```C#
//获取父物体
transform.parent.gameObject
//子物体个数
transform.childCount
//解除与子物体的父子关系
transform.DetachChildren();
//获取子物体
Transform trans = transform.Find("Child");
//获取第零个子物体
transfoem.GetChild(0)
//判断一个物体是不是另一个物体的子物体
bool res = trans.IsChildOd(transform);
//设置为父物体
trans.SetParent(transform);
```
## 交互操作
### PC操作方式（键鼠操作）
键鼠事件需要时刻监听，故需写在update中
```C#
void Update()
{
    //鼠标的点击
    //按下鼠标 0左键 1右键 2滚轮
    if(Input.GetMouseButtonDown(0){
        Debug.log("按下了鼠标左键")；
    })
    if(Input.GetMouseButton(0){
        Debug.log("持续按下鼠标左键")；
    })
    if(Input.GetMouseButtonup(0)){
        Debug.log("抬起鼠标左键");
    }

    //键盘操作
    //按下键盘按键
    Input.GetKeyDown(KeyCode.A)
    //持续按下按键
    Input.GetKey(KeyCode.A)
    //抬起键盘按键
    Input.GetKeyUp(KeyCode.A)
    //(KeyCode.A)--("a")

}
```
### 虚拟轴
简单来说，虚拟轴就是一个数值在(-1,1)内的数轴，这个数轴上重要的数值就是-1、0和1。当使用按键模拟一个完整的虚拟轴时需要用到两个按键，即将按键1设置为负轴按键，按键2设置为正轴按键。在没有按下任何按键的时候，虚拟轴的数值为0；在按下按键1的时候，虚拟轴的数值会从(0,-1)进行过渡；在按下按键2的时候，虚拟轴的数值会从0~1进行过渡，如图所示。  
![虚拟轴](pic/U3.png)  
虚拟轴的存在可以整合各个操作设备不同的操作方式  
```C#
void Update()
{
    //获取水平轴
    float horizontal = Input.GetAxis("Horizontal")；
    float vertical = Input.GetAxis("Vertical")；
    Debug.Log(horizontal + "   "+ vertical );
    //虚拟按键
    if(Input.GetButtonDown("Jump"))
    {
        Debug.log("空格")；
    }
    /* 同上节各种按键触发方式均可类比实现
    */
}
```
### 触摸方法
单点触摸/多点触摸
```C#
//开启多点触摸
Input.multiTouchEnabled = true

//判断单点触摸
if(Input.touchCount == 1)
{
    //触摸对象
    Touch touch = Input.touches[0];
    //触摸位置
    Debug.log(touch.position);
    //触摸阶段
    switch (touch.phase)
    {
        case TouchPhase.Began;
        break;
        case TouchPhase.Moved;
        break;
        case TouchPhase.Stationary;
        break;
        case TouchPhase.Ended;
        break;
        case TouchPhase.Canceled;
        break;
    }
}

//判断多点触摸
if(Input.touchCount == 2)
{
    Touch touch = Input.touches[0];
    Touch touch1 = Input.touches[1];
    //
}
```
## 场景布局  
### 灯光
定向灯光的照明效果只和方向有关，与位置无关。——模拟太阳光照
聚光——手电筒  
点光——灯泡  
实时/烘培————实时光源时刻计算，烘培固定不变（类似光照贴图）
>ps：建议动手尝试烘培功能    
### 摄像机  
* 透视摄像机：近大远小，与现实相同  
* 正交摄像机：不论距离，画面大小不变  
>ps：关于摄影机画面的叠加效果需要动手尝试理解。  
### 声音的使用  
audio lisener组件————挂在摄影机上，好像一般只挂一个？    
Audio Source组件  
>ps：上手就会了，无需多盐  
 相关的方法
 ```C#
public AudioClip music;
public AudioClip se;

//播放器组件
private AudioSouce player;
void Start(){
    player = GetComponent<AudioSource>();
    //设定播放的音频片段
    play.clip = music;
    //循环
    player.loop = true;
    //音量
    player.volume = 0.5f;
    //播放
    player.Play();

}
void Update()
{
    //空格切换播放与暂停
    if(Input.GetKeyDown(KeyCode.Space)){
        if(play.isPlaying){
            //暂停
            play.Pause();
            //停止
            play.Stop();
        }else{
            //继续
            play.Unpause();
            //开始播放
            player.Play();
        }
        //鼠标左键播放音效
        if(Input.GetMouseButtonDown(0)){
            play.PlayOneShot(se);
        }
    }
}
 ```
>ps：暂停与继相对应，停止与开始，后者的区别是直接结束音乐从头开始。如果是写一些音效的触发直接写一个按键触发播放的效果就行了。  
### 视频的使用  
video player————>渲染器纹理  
UI————>原始图像————>渲染器纹理  
```C#
private VideoPlayer player;
//player = GetComponent<VideoPlayer>();
```
>ps：与音频类似  
### 角色控制器  
```C#
private CharaterController player;
void Start(){
    player = GetComponent<CharaterController>();
}
void Update()
{
    //水平轴
    float horizontal = Input.GetAxis("Horizontal");
    //垂直轴
    float vertical = Input.GetAxis("vertical");
    //创建成一个方向向量
    Vector3 dir = new Vector3(horizontal,0,vertical);
    //朝向该方向移动
    play.SimpleMove(dir);
}
```  
## 物理系统
### 物理系统  
* rigidbody组件：赋予物体刚体的物理属性，可选择开启重力效果，运动学效果；碰撞检测的可选项：离散型可能对高速物体丢失检测；冻结选项：字面意思。  
### 碰撞检测
* 碰撞器（Collider）：线框决定碰撞范围，产生碰撞需要有一个物体具有刚体属性
```C#
//监听发生碰撞
private void OnCollisionEnter(Collision collision)
{
    //创建一个爆炸物体
    Instantiate(Prefab,transform.position,Quaternion.identity);
    //销毁自身
    Destroy(gameObject);
    //获取碰撞到的游戏物体
    Debug.log("collision.gameObject.name");
}
//持续碰撞
private void OnCollisionStay(Collision collision)
{

} 
//结束碰撞
private void OnCollisionExit(Collision collision)
{

}
```  
### 触发效果  
```C#
void Update()
{
    //水平轴
    float Horizontal = Input.GetAxis("Horizontal");
    //垂直轴
    float vertical = Input.GetAxis("Vertical");
    //向量
    Vector3 dir = new Vector3(horizontal,0,vertical);
    //朝向量方向移动
    transform.Translate(dir*2*Time.deltaTime);
}
```  
触发器：collider中的触发器选项，打开后其他物体可以穿过该物体  
```C#
private void OnTriggerEnter(Collider other)
{
    GameObject door = GameObject.Find("Door");
    if(door==null)
    {
        door.SetActive(false);
    }
}
private void OnTriggerStay(Collider other)
{

}
private void OnTriggerExit(Collider other)
{

}
```  
### 物理关节 
* 铰链（Hinge Joint）：绕轴旋转--设定轴的位置--x、y偏移值--速度&力：自动旋转   
* 弹簧（Spring Joint）：链接在刚体上面，弹簧效果  
* 固定关节（Fixed Joint）：将物体1固定到物体2上，物体1无法移动，物体2移动则物体1同步动--设定一个力的阈值冲破关节  
* ......  
### 物理材质 
自带的物理材质设置动态静态摩擦力等，这部分教的比较简单，毕竟侧重点不在这一块
### 射线检测  
通过摄像机从二维平面射向场景  
实现：
```C#
void Start()
{
    //方式1
    Ray ray = Ray(Vector3.zero,Vector3.up);

}
void Update()
{
    //方式2
    if(Input.GetMouseButtonDown(0))
    {
        //按下鼠标左键发射射线
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        //声明一个碰撞信息类
        RaycastHit hit;
        //碰撞检测
        bool res = Physics.Raycast(ray,out hit);
        //若碰撞到物体，则hit返回内容
        if(res == true)
        {
            Debug.log(hit.point);
            transform.position = hit.point;
        }
        //多检测
        //RaycastHit[] hits = Physics.RaycastAll(ray,100,1<<10);
    }
}
```  
### 粒子系统的基本使用  
* 粒子系统（Particle System）:时间、速度、形状……  
* 预热：去除部分周期，直接展示完整的
>ps：需要用的时候再好好看看，目前都是一些基本的编辑器操作  
### 线条与拖尾效果  
* Line 物体：位置，顶点数（决定平滑度），循环......  
相关方法  
```C#
void Start()
{
    //设点线段位置
    //获取线段渲染器
    LineRenderer lineRenderer = GetComponent<LineRenderer>();
    LineRenderer.positionCount = 3;
    LineRenderer.SetPosition(0,Vector3.zero);
    LineRenderer.SetPosition(0,Vector3.one);
    LineRenderer.SetPosition(0,Vector3.down);
    //lineRenderer.setp
}
``` 
* 拖尾（Trail）:类似的编辑器操作，物体移动产生拖尾效果，设定相关时间和显示效果等  
## 动画  
### 基础操作
* Animation（旧）：Clip，自动播放，是否总是动画化；
创作动画--K帧；
可以通过写脚本去控制动画的触发
* Animator（新）：控制器，Avatar，  
控制器即动画的状态机加行动树的控制，可以控制动画的播放状态，可以通过脚本去操作；  
如
```C#
private Animator animator;
void Start()
{
    //获取动画器组件
    animator = GetComponent<Animator>();
}
void Update()
{
    if(Input.GetMouseButtonDown(0))
    {
        animator.Play("right");
    }
}
```  
可以通过在状态机中添加触发器结合代码管理一些动画操作
```C#
void Update()
{
    if(Input.GetKeyDown(KeyCode.F))
    {
        //触发pickup参数
        GetComponent<Animator>().SetTrigger("pickup");
    }
}
```
过渡：又退出时间时当你触发转换动画时会播放完当前动画后切换，关闭后可以即时切换  
### 按键控制角色运动  
```C#
void Update()
{
    //水平轴
    float horizontal = Input.GetAxis("Horizontal");
    //垂直轴
    float vertical = Input.GetAxis("Vertical");
    //向量
    Vector3  dir = new Vector3(horizontal , 0, vertical);
    //当用户按下方向键
    if(dir! = Vector3.zero)
    {
        //面向向量
        transform.rotation = Quaternion.LookRotation(dir);
        //播放跑步动画
        animator.SetBool("IsRun",true);
        //朝向前方移动
        transform.Translate(Vector3.forward * 2 * Time.deltaTime);
    } else 
    {
        //播放站立动画
        animator.SetBool("IsRun",false);
    }
}
```
>ps：注意去除过渡持续时间，否则容易出现延迟效果
### 曲线和帧事件  
* 曲线：提出一个曲线，其中的数值可以提取出来用于控制其他动画或效果；  
* 帧事件：设定一个时刻设置事件，动画每次到这一帧时执行脚本   

二者主要用于动画与其他动画、特效之间的配合与协同  
### 混合动画  
* Blend Tree的使用
* 走路跑步混合--参数权重  
### 动画分层  
* 每个图层具有三种状态：Any State、Entry、Exit  
* 遮罩功能的使用：可以限定动画播放的范围，如用于同一个人物同时做不同的动作的动画混合  
### 动捕方案  
千面动捕  
做独游可以考虑的低成本方案   
后面再研究吧  
## Inverse Kinematics
### 反向动力学
* 概念：与正向相反，如我们手的动作本来是从胳膊到手臂到手指，现在我们通过手指的位置反推手臂与胳膊  
```C#
private void OnAnimatorIK(int layerIndex)
{
    //设置头部IK,固定看向某个方向
    animator.SetLookAtWeight(1);
    animator.SetLookAtPosition(target.position);
    //设置右手IK权重
    animator.SetIKPositionWeight(AvatarIKGoal.RightHand,1);
    //是否影响旋转
    animator.SetIKRotationWeight(AvatorIKGoal.RightHand,1);
    //设置右手IK
    animator.SetIKPosition(AvatarIKGoal.RightHand，target.position);
    animator.SetIKPosition(AvatarIKGoal.RightHand，target.rotation);
}
``` 
商城有现成的ik资源  
## 导航 
### Navigation Static 
窗口--包管理器--Unity注册表--Ai Navigation--安装  
上手就会了  
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Al;

public class PlayerControl : MonoBehaviour
private NavMeshAgent agent;
void Start()
{
    //获取代理组件
    agent = GetComponent<NavMeshAgent>0;
}
void Update()
{
    //如果按下鼠标
    if (nput.GetMouseButtonDown(o))
    {
        //获取点击位置
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        RaycastHit hit;
        if (Physics.Raycast(ray, out hit))
        {
            //点击位置
            Vector3 point = hit.point;
            //设置该位置为导航目标点
            agent.SetDestination(point);
        }        
    }    
}
    
```  
上述代码实现了一个类似LOL的移动系统
### 动态障碍物
将其属性取消NS  
切割属性的使用--自动门  
网格链接--跳跃高度--跳跃距离  
### 导航区域
可以设置不同区域与区域权重    

## UI系统
核心组件：画布、事件系统  
* 渲染模式：  
覆盖————始终显示摄像机所拍摄的画面的前方，即最前方  
摄像机————需要有摄像机，可以显示在物体前后  
世界空间————需要摄像机，可以进行旋转缩放等，前两者始终面向摄像机  
* 排序次序  
* UI缩放模式  
### 锚点与轴心点  
* Image ：都是一些常规功能，上手就会 
* 锚点：固定在父物体身上， 图像的位置即相对于锚点的偏移量
>ps：多个锚点时，锚点变动时，锚点内的物体等比例缩放，锚点外固定大小
* 轴心点：轴心点就是用于计算物体与画布相对位置与旋转的一个点，固定在物体（图像）上
## 文本功能  
都是一些比较简单的方法，具体做UI的时候再来打磨，教程里只介绍了一些简单的编辑器操作
### 按钮  

### 文本框  

### 选项与下拉框  

### 滚动条与滚动视图  

### 面板与布局  
