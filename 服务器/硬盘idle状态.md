硬盘有4个电源状态，分别是

Active
Idle
Standby
Sleep

硬盘在使用过程中，如果不进行读写，他会自动进入idle(空闲)状态，而idle状态又分三个等级，idle_A，idle_B, idle_C，在客户多样化的应用环境中，产品中硬盘的性能会严重影响到客户的整体应用。

硬盘在开机后，不进行读写使用时，2mins后会自动进入idle A状态，降低电量；1mins后会进入idle B状态，磁头停止工作，磁盘全速转动；30mins后会进入idle C状态，磁头停止工作，磁盘减速转动;60mins后进入Standby_Z状态，磁头停止工作，磁盘停转。但这些状态都是自然静置时进入的。

查看硬盘状态

smartctl -i -n standby /dev/sdb|grep "mode"|awk '{print $4}'
