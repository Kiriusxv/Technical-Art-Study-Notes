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
简单来说，虚拟轴就是一个数值在-1~1内的数轴，这个数轴上重要的数值就是-1、0和1。当使用按键模拟一个完整的虚拟轴时需要用到两个按键，即将按键1设置为负轴按键，按键2设置为正轴按键。在没有按下任何按键的时候，虚拟轴的数值为0；在按下按键1的时候，虚拟轴的数值会从0~-1进行过渡；在按下按键2的时候，虚拟轴的数值会从0~1进行过渡，如图所示。  
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