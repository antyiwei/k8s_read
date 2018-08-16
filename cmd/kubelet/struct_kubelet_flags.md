# KubeletFlags 结构细说

```go
const defaultRootDir = "/var/lib/kubelet"

//如果满足以下任何条件，配置字段应该放在KubeletFlags而不是KubeletConfiguration中：
//  - 在节点的生命周期中，它的值永远不会或不能安全地更改
//  - 无法在节点之间安全地共享其值（例如主机名）
// KubeletConfiguration旨在在节点之间共享
//一般情况下，请尽量避免添加标记或配置字段，
//我们已经有了大量令人困惑的东西。
type KubeletFlags struct {
	KubeConfig          string
	BootstrapKubeconfig string

	// 在调用主服务器期间插入随机错误的概率.
	ChaosChance float64
	// 立刻崩溃，而不是吃恐慌.
	ReallyCrashForTesting bool

	// TODO(mtaufen): 越来越多的人看起来没有人真正使用过  Kubelet的runonce模式已经存在，因此它可能是弃用和删除的候选者。
	//    如果runOnce为true，则Kubelet将检查API服务器一次是否为pod，运行除静态pod文件指定的pod之外的那些，并退出.
	RunOnce bool

	// enableServer启用Kubelet的服务器
	EnableServer bool

	// HostnameOverride是用于标识kubelet而不是实际主机名的主机名.
	HostnameOverride string
	// NodeIP是节点的IP地址。如果设置，则kubelet将使用此IP地址作为节点.
	NodeIP string

	// 此标志（如果已设置）设置外部提供程序（即cloudprovider）可用于标识特定节点的实例的唯一ID
	ProviderID string

	// Container-runtime-specific options.
	config.ContainerRuntimeOptions

	// certDirectory是TLS证书所在的目录（默认情况下为/var/run/kubernetes）。如果提供了tlsCertFile和tlsPrivateKeyFile，则将忽略此标志。
	CertDirectory string

	// cloudProvider是云服务的提供商。 +可选
	CloudProvider string

	// cloudConfigFile是云提供程序配置文件的路径。 +可选
	CloudConfigFile string

	// rootDirectory是放置文件夹文件的目录路径（卷安装等）.
	RootDirectory string
	
	// Kubelet将使用此目录检查下载的配置并跟踪配置运行状况。
	// Kubelet将创建此目录（如果该目录尚不存在）。
	// 路径可以是绝对的或相对的;相对路径位于Kubelet的当前工作目录下。 
	// 提供此标志可启用动态kubelet配置。 
	// 要使用此标志，必须启用DynamicKubeletConfig功能门.
	DynamicConfigDir flag.StringFlag
	
	// Kubelet将从此文件加载其初始配置。
	// 路径可以是绝对的或相对的;相对路径位于Kubelet的当前工作目录下。 
	// 省略此标志以使用内置默认配置值和标志的组合。
	KubeletConfigFile string

	// registerNode启用自动注册apiserver.
	RegisterNode bool

	// registerWithTaints是当kubelet注册自身时要添加到节点对象的taints数组。这仅在registerNode为true且初始注册节点时生效。
	RegisterWithTaints []core.Taint

	// WindowsService 如果kubelet在Windows上作为服务运行，则应将WindowsService设置为true。其相应的标志仅在Windows版本中注册.
	WindowsService bool
	
	//=============================华丽的分割线=======================================================

	// EXPERIMENTAL FLAGS 实验标志
	//  不安全的sysctl或sysctl模式的白名单（以*结尾）。 +可选
	AllowedUnsafeSysctls []string
	// containerized 如果kubelet在容器中运行，则应将containerized设置为true.
	Containerized bool
	// remoteRuntimeEndpoint is the endpoint of remote runtime service
	// 译：remoteRuntimeEndpoint是远程运行时服务的端点
	RemoteRuntimeEndpoint string
	// remoteImageEndpoint is the endpoint of remote image service
	// 译：remoteImageEndpoint是远程图像服务的端点
	RemoteImageEndpoint string
	// experimentalMounterPath is the path of mounter binary. Leave empty to use the default mount path
	// 译：experimentalMounterPath是mounter二进制文件的路径。保留为空以使用默认装载路径
	ExperimentalMounterPath string
	// 译：如果启用，则kubelet将与内核memcg通知集成，以确定是否超过了内存逐出阈值而不是轮询。  +可选
	ExperimentalKernelMemcgNotification bool
	// This flag, if set, enables a check prior to mount operations to verify that the required components
	// (binaries, etc.) to mount the volume are available on the underlying node. If the check is enabled
	// and fails the mount operation fails.
	// 译：此标志（如果已设置）在安装操作之前启用检查，以验证安装卷所需的组件（二进制文件等）在基础节点上是否可用。如果启用了检查并且失败，则安装操作将失败
	ExperimentalCheckNodeCapabilitiesBeforeMount bool
	// This flag, if set, will avoid including `EvictionHard` limits while computing Node Allocatable.
	// Refer to [Node Allocatable](https://git.k8s.io/community/contributors/design-proposals/node-allocatable.md) doc for more information.
	// 译：如果设置了此标志，则在计算Node Allocatable时将避免包含`EvictionHard`限制。 
	// 译：有关详细信息，请参阅[Node Allocatable]（https://git.k8s.io/community/contributors/design-proposals/node-allocatable.md）doc。
	ExperimentalNodeAllocatableIgnoreEvictionThreshold bool
	// NodeLabel是在群集中注册节点时要添加的节点标签
	NodeLabels map[string]string
	// volumePluginDir是搜索其他第三方卷插件的目录的完整路径
	VolumePluginDir string
	// lockFilePath是kubelet用作锁定文件的路径。它使用此文件作为锁，以与可能正在运行的其他kubelet进程同步
	LockFilePath string
	// ExitOnLockContention是一个标志，表示kubelet正在运行在“bootstrap”模式下。这需要设置'LockFilePath'。这将导致kubelet监听锁定文件上的inotify事件，释放它并在另一个进程尝试打开该文件时退出。
	ExitOnLockContention bool
	// seccompProfileRoot是seccomp配置文件的目录路径.
	SeccompProfileRoot string
	// bootstrapCheckpointPath是包含要在还原时运行的pod检查点的目录的路径。
	BootstrapCheckpointPath string
	// NodeStatusMaxImages会限制Node.Status.Images中报告的图像数量。这是一个实验性的短期标志，有助于节点可扩展性。
	NodeStatusMaxImages int32
	
	//=============================华丽的分割线=======================================================

	// DEPRECATED FLAGS 弃用
	// minimumGCAge是成品容器在收集垃圾之前的最小年龄.
	MinimumGCAge metav1.Duration
	// maxPerPodContainerCount是每个容器保留的最大旧实例数。每个容器占用一些磁盘空间.
	MaxPerPodContainerCount int32
	//maxContainerCount是全局保留的旧容器实例的最大数量。每个容器占用一些磁盘空间
	MaxContainerCount int32
	// masterServiceNamespace是应该将kubernetes主服务注入pod的命名空间.
	MasterServiceNamespace string
	// registerSchedulable告诉kubelet将节点注册为可调度的。如果register-node为false，则不会有任何影响。 DEPRECATED：改为使用registerWithTaints
	RegisterSchedulable bool
	// nonMasqueradeCIDR configures masquerading: traffic to IPs outside this range will use IP masquerade.
	NonMasqueradeCIDR string
	// 此标志（如果已设置）指示kubelet保持已终止的pod安装到节点的卷。这对于调试与卷相关的问题非常有用。
	KeepTerminatedPodVolumes bool
	// allowPrivileged使容器能够请求特权模式。默认为true。
	AllowPrivileged bool
	// hostNetworkSources是一个以逗号分隔的源列表Kubelet允许pod使用主机网络。默认为“*”。有效选项是“file”，“http”，“api”和“*”（所有来源）。
	HostNetworkSources []string
	// hostPIDSources是一个以逗号分隔的源列表，Kubelet允许pod使用主机pid命名空间。默认为“*”.
	HostPIDSources []string
	// hostIPCSources是一个以逗号分隔的源列表，Kubelet允许pod使用host ipc命名空间。默认为“*”.
	HostIPCSources []string
}

// NewKubeletFlags将使用默认值创建一个新的KubeletFlags
func NewKubeletFlags() *KubeletFlags {
	remoteRuntimeEndpoint := ""
	if runtime.GOOS == "linux" {
		remoteRuntimeEndpoint = "unix:///var/run/dockershim.sock"
	} else if runtime.GOOS == "windows" {
		remoteRuntimeEndpoint = "tcp://localhost:3735"
	}

	return &KubeletFlags{
		EnableServer:                        true,
		ContainerRuntimeOptions:             *NewContainerRuntimeOptions(),
		CertDirectory:                       "/var/lib/kubelet/pki",
		RootDirectory:                       defaultRootDir,
		MasterServiceNamespace:              metav1.NamespaceDefault,
		MaxContainerCount:                   -1,
		MaxPerPodContainerCount:             1,
		MinimumGCAge:                        metav1.Duration{Duration: 0},
		NonMasqueradeCIDR:                   "10.0.0.0/8",
		RegisterSchedulable:                 true,
		ExperimentalKernelMemcgNotification: false,
		RemoteRuntimeEndpoint:               remoteRuntimeEndpoint,
		NodeLabels:                          make(map[string]string),
		VolumePluginDir:                     "/usr/libexec/kubernetes/kubelet-plugins/volume/exec/",
		RegisterNode:                        true,
		SeccompProfileRoot:                  filepath.Join(defaultRootDir, "seccomp"),
		HostNetworkSources:                  []string{kubetypes.AllSource},
		HostPIDSources:                      []string{kubetypes.AllSource},
		HostIPCSources:                      []string{kubetypes.AllSource},
		// TODO(#58010:v1.13.0): Remove --allow-privileged, it is deprecated
		AllowPrivileged: true,
		// prior to the introduction of this flag, there was a hardcoded cap of 50 images
		NodeStatusMaxImages: 50,
	}
}
```
