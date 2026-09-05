# Unity教程

### 第一部分：环境安装与项目创建

- 
**安装 Unity Hub 与编辑器**

**下载 Unity Hub**：访问 [Unity 官网](https://unity.com/) 下载最新版本。

- 
**安装 Unity 编辑器**：

打开 Unity Hub → 选择 **Installs → Install Editor**。

- 
建议选择 **2021 LTS 或更高版本**（稳定且兼容性强）。

- 
安装时勾选 **Windows/Mac Build Support**（根据开发平台选择），若需移动端开发可勾选 **Android/iOS** 模块。

- 
**配置外部工具**：在 Unity 首选项（Preferences → External Tools）中设置 Visual Studio 或 VSCode 为默认代码编辑器。

- 
**创建新项目**

选择 **3D 模板**（适合新手）或 **2D 模板**（如制作平台跳跃游戏）。

- 
命名项目（如 `MyFirstGame`），选择无中文的存储路径。

- 
首次启动后，熟悉默认生成的场景结构（包含主摄像机 `Main Camera` 和方向光 `Directional Light`）。

---

### 第二部分：编辑器界面深度解析

- 
**核心面板功能**

**Scene 视图**：3D/2D 场景编辑区，支持快捷键操作：

**移动视角**：鼠标右键拖拽。

- 
**平移场景**：按住鼠标中键拖拽。

- 
**聚焦物体**：选中物体后按 `F` 键。

- 
**Game 视图**：预览游戏运行效果，可调整分辨率（如 16:9 的 1920×1080）。

- 
**Hierarchy 面板**：管理场景中的物体层级，支持父子关系（子物体继承父物体的变换）。

- 
**Inspector 面板**：显示选中物体的属性和组件，例如 `Transform`（位置、旋转、缩放）、`Rigidbody`（刚体）等。

- 
**Project 面板**：资源管理器，按类型分类存放脚本、材质、模型等。

- 
**视图布局优化**

切换布局：顶部菜单 **Window → Layouts**，选择 `2D`、`3D` 或自定义布局。

- 
调整面板大小：拖动面板边缘，保存个人布局（**Window → Layouts → Save Layout**）。

---

### 第三部分：基础游戏对象操作与脚本编写

- 
**创建与配置游戏对象**

**地面与玩家角色**：

右键 Hierarchy → `3D Object → Plane` 创建地面，调整 `Scale` 至 (5,1,5)。

- 
创建 `Capsule` 作为玩家，设置 `Position` 为 (0,1,0) 避免陷入地面。

- 
**刚体与碰撞体**：

为玩家添加 `Rigidbody` 组件（Inspector → Add Component → Physics → Rigidbody）。

- 
为收集物（如 Cube）添加 `Box Collider` 并勾选 `Is Trigger`，用于触发检测。

- 
**C# 脚本控制移动**

**创建脚本**：Project 面板右键 → `Create → C# Script`，命名为 `PlayerController`。

- 
**代码逻辑**：

```
using UnityEngine;

public class PlayerController : MonoBehaviour
{
 public float moveSpeed = 5f;
 private Rigidbody rb;

 void Start()
 {
     rb = GetComponent();
 }

 void FixedUpdate()
 {
     float horizontal = Input.GetAxis("Horizontal");
     float vertical = Input.GetAxis("Vertical");
     Vector3 movement = new Vector3(horizontal, 0f, vertical) * moveSpeed * Time.fixedDeltaTime;
     rb.MovePosition(rb.position + movement);
 }

 void OnTriggerEnter(Collider other)
 {
     if (other.CompareTag("Collectible"))
     {
         Destroy(other.gameObject);
         // 后续添加计分逻辑
     }
 }
}
```

- 
**脚本附加与调试**：

拖拽脚本到玩家物体，确保 `Rigidbody` 组件存在。

- 
使用 VSCode 调试：安装 `Unity Debugger` 扩展，配置 `launch.json` 附加到 Unity 进程。

---

### 第四部分：物理与交互系统进阶

- 
**物理引擎配置**

**刚体属性**：调整 `Mass`（质量）、`Drag`（阻力）优化移动手感。

- 
**碰撞层设置**：通过 `Layer Collision Matrix` 控制不同层物体是否碰撞（Edit → Project Settings → Physics）。

- 
**UI 系统与计分功能**

**创建 UI 文本**：

Hierarchy 右键 → `UI → Text`，重命名为 `ScoreText`。

- 
调整 Canvas 的渲染模式为 `Screen Space - Overlay`，设置文本位置、字体大小。

- 
**计分脚本**：

```
public class ScoreManager : MonoBehaviour
{
 public static ScoreManager Instance;
 public Text scoreText;
 private int score = 0;

 void Awake() => Instance = this;

 public void AddScore(int points)
 {
     score += points;
     scoreText.text = $"Score: {score}";
 }
}
```

- 
在玩家脚本的 `OnTriggerEnter` 中调用 `ScoreManager.Instance.AddScore(10);`。

---

### 第五部分：优化与发布

- 
**性能优化**

**减少 Draw Calls**：合并材质，使用纹理图集。

- 
**对象池技术**：复用游戏对象（如子弹、敌人），避免频繁实例化与销毁。

- 
**跨平台发布**

**构建设置**：File → Build Settings → 选择目标平台（如 PC、Android）。

- 
**分辨率适配**：调整 Canvas Scaler 的 `UI Scale Mode` 为 `Scale With Screen Size`。

- 
**调试技巧**

**断点调试**：在 VSCode 中设置断点，运行游戏后触发代码暂停。

- 
**日志输出**：使用 `Debug.Log()` 输出变量值，在 Console 面板查看。

---

### 常见问题与解决方案

- 
**角色移动卡顿**

确保在 `FixedUpdate` 中处理物理移动，而非 `Update`。

- 
**碰撞检测失效**

检查碰撞体的 `Is Trigger` 是否误开启，或调整刚体的 `Collision Detection` 为 `Continuous`。

- 
**脚本未生效**

确认类名与文件名一致，且脚本已附加到正确物体。

---

### 实战案例扩展

- 
**2D 平台跳跃游戏**

使用 `Sprite Renderer` 与 `Tilemap` 创建地形。

- 
添加 `CharacterController2D` 组件处理跳跃与碰撞。

- 
**篝火交互系统**

导入火焰特效资源，编写脚本通过按键触发粒子效果：

```
if (Input.GetKeyDown(KeyCode.F)) 
 fireEffect.SetActive(true);
```

---

### 第六部分：详细项目开发教程——《3D 平台跑酷游戏》

本教程将指导你从头开发一个完整的 **3D 平台跑酷游戏**，包含角色控制、关卡设计、敌人AI、UI系统、音效与存档功能。目标是让玩家跳跃障碍、收集金币、击败敌人并抵达终点。

---

### 1. 项目规划

- 
**核心玩法设计**

玩家使用键盘控制角色跳跃、冲刺。

- 
场景中有移动平台、陷阱、金币和巡逻的敌人。

- 
游戏包含多关卡，支持存档读档。

- 
**资源准备**

**角色模型**：从 [Mixamo](https://www.mixamo.com/) 下载免费角色动画。

- 
**场景素材**：使用 Unity Asset Store 的免费资源（如 "Simple City Pack"）。

- 
**音效与音乐**：访问 [Freesound](https://freesound.org/) 下载跳跃、金币收集音效。

---

### 2. 场景搭建

- 
**地形与光照**

创建地形：菜单栏 **GameObject → 3D Object → Terrain**，使用笔刷刷出高低起伏。

- 
添加天空盒：Window → Rendering → Lighting → Environment → Skybox Material。

- 
设置动态光源：添加 **Point Light** 在场景关键位置，启用阴影（Shadow Type → Soft Shadows）。

- 
**平台与障碍物**

创建移动平台：使用 Cube 物体，附加脚本使其循环移动：

```
public class MovingPlatform : MonoBehaviour
{
 public float speed = 2f;
 public Vector3[] waypoints;
 private int currentWaypoint = 0;

 void Update()
 {
     if (Vector3.Distance(transform.position, waypoints[currentWaypoint]) 
- 
添加尖刺陷阱：使用多个细长 Cube 组合成尖刺形状，附加 **Box Collider** 并编写伤害逻辑。

---

### 3. 角色控制进阶

- 
**角色动画与状态机**

导入 Mixamo 角色模型与动画（Idle, Run, Jump）。

- 
创建 Animator Controller：右键 Project 面板 → **Create → Animator Controller**。

- 
设置动画状态机：

**状态过渡**：Idle → Run（条件：`Speed > 0.1`）、Run → Jump（条件：`IsJumping == true`）。

- 
代码控制动画参数：

```
public class PlayerController : MonoBehaviour
{
private Animator animator;
private bool isGrounded;

void Start() => animator = GetComponent();

void Update()
{
   float moveInput = Input.GetAxis("Horizontal");
   animator.SetFloat("Speed", Mathf.Abs(moveInput));

   if (Input.GetButtonDown("Jump") && isGrounded)
       animator.SetTrigger("Jump");
}

void OnCollisionEnter(Collision collision)
{
   if (collision.gameObject.CompareTag("Ground"))
       isGrounded = true;
}
}
```

- 
**跳跃与冲刺**

跳跃逻辑：

```
public float jumpForce = 5f;
private Rigidbody rb;

void Jump()
{
 if (isGrounded)
 {
     rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
     isGrounded = false;
 }
}
```

- 
冲刺功能（按 Shift 加速）：

```
public float dashSpeed = 10f;
void Dash()
{
 if (Input.GetKeyDown(KeyCode.LeftShift))
     rb.AddForce(transform.forward * dashSpeed, ForceMode.Impulse);
}
```

---

### 4. 敌人AI与战斗系统

- 
**敌人巡逻逻辑**

创建敌人预制体：使用 Capsule 作为基础形状，附加 **NavMeshAgent** 组件。

- 
烘焙导航网格：菜单栏 **Window → AI → Navigation** → Bake。

- 
编写巡逻脚本：

```
public class EnemyAI : MonoBehaviour
{
 public Transform[] patrolPoints;
 private NavMeshAgent agent;
 private int currentPoint = 0;

 void Start() => agent = GetComponent();

 void Update()
 {
     if (agent.remainingDistance 

- 
**玩家攻击与敌人死亡**

添加攻击键（如鼠标左键），检测前方敌人：

```
void Attack()
{
 RaycastHit hit;
 if (Physics.Raycast(transform.position, transform.forward, out hit, 2f))
 {
     if (hit.collider.CompareTag("Enemy"))
         Destroy(hit.collider.gameObject);
 }
}
```

- 
敌人死亡时播放粒子特效：

```
public GameObject deathEffect;
void Die()
{
 Instantiate(deathEffect, transform.position, Quaternion.identity);
 Destroy(gameObject);
}
```

---

### 5. UI 系统与存档功能

- 
**游戏菜单与HUD**

创建主菜单：添加 **Canvas**，包含按钮（Play, Options, Quit）。

- 
暂停菜单：按 Esc 键弹出，使用 `Time.timeScale = 0` 暂停游戏。

- 
**数据持久化**

使用 `PlayerPrefs` 存储最高分与关卡进度：

```
public class SaveSystem : MonoBehaviour
{
 public static void SaveHighScore(int score)
 {
     PlayerPrefs.SetInt("HighScore", score);
     PlayerPrefs.Save();
 }

 public static int LoadHighScore()
 {
     return PlayerPrefs.GetInt("HighScore", 0);
 }
}
```

---

### 6. 音效与优化

- 
**音效管理**

创建全局 AudioManager：

```
public class AudioManager : MonoBehaviour
{
 public static AudioManager Instance;
 public AudioSource bgmSource, sfxSource;

 void Awake() => Instance = this;

 public void PlaySFX(AudioClip clip)
 {
     sfxSource.PlayOneShot(clip);
 }
}
```

- 
在收集金币时调用：

```
AudioManager.Instance.PlaySFX(coinSound);
```

- 
**性能优化**

**遮挡剔除（Occlusion Culling）**：减少不可见物体的渲染。

- 
**LOD Group**：为复杂模型添加多级细节（Window → Rendering → LOD Group）。

---

### 7. 构建与发布

- 
**跨平台设置**

切换平台：**File → Build Settings** → 选择 Android/iOS/PC。

- 
安卓配置：安装 JDK、Android SDK，设置 Bundle Identifier。

- 
**生成APK/EXE**

点击 **Build**，选择输出路径，等待编译完成。

---

### 项目扩展方向

- 
**多人联机功能**

使用 Unity Netcode 或 Photon PUN 实现多人合作。

- 
**剧情与任务系统**

添加对话系统（如使用 Ink 叙事脚本）与任务追踪UI。

- 
**Steam集成**

集成 Steamworks SDK 实现成就与云存档。

---

通过本教程，你不仅能掌握 Unity 的基础操作，还能深入理解物理、UI、调试等进阶功能。建议结合 Unity 学习路线图 系统化学习，并参考官方文档与社区资源（如 Unity Learn 平台）持续提升！
