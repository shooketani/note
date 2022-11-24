## Technical Verification
### テーマ
「マイクロサービスアーキテクチャにおけるカオスエンジニアリング製品の検証・評価」

### カオスエンジニアリング製品
#### AWS Fault Injection Simulator
- 以下のFault Injectionを発生させることができる。 （[AWS Fault Injection Simulator を早速使ってみました](https://qiita.com/hirosys-biz/items/16002428f87c08c0a637)）
  - EC2 インスタンスの再起動(aws:ec2:reboot-instances)
  - EC2 インスタンスの停止(aws:ec2:stop-instances)
  - EC2 インスタンスの終了(削除)(aws:ec2:terminate-instances)
  - ECS のコンテナインスタンスのドレイニング(aws:ecs:daion-container-instances)
  - EKS のノードグループインスタンスの削除(aws:eks:terminate-nodegroup-instances)
  - AWS サービスの応答をエラーにする(aws:fis:injection-api-internal-error)
  - AWS サービスの応答をスロットルエラーにする(aws:fis:injection-api-throttle-error)
  - AWS サービスの応答を利用不可エラーにする(aws:fis:injection-api-unavailable-error)
  - 実験にWaitを入れる(aws:fis:wait)
  - RDS の DB クラスターをフェールオーバーする(aws:rds:failover-db-cluster)
  - RDS の DB インスタンスを再起動する(aws:rds-reboot-db-instances)
  - SSM ドキュメントを実行する(コマンドを実行する)(aws:ssm:send-command)
  - EC2 インスタンスに対して CPU 負荷を与える(aws:ssm:send-command/AWSFIS-Run-CPU-Stress)
  - EC2 インスタンス内のプロセスを Kill する(aws:ssm:send-command/AWSFIS-Run-Kill-Process)
  - EC2 インスタンスに対してメモリ負荷を与える(aws:ssm:send-command/AWSFIS-Run-Memory-Stress)
  - EC2 インスタンスに対してネットワーク負荷を与える(aws:ssm:send-command/AWSFIS-Run-Network-Latency)

#### Gremlin
- 以下の環境にインストール可能 [Gremlin Docs](https://www.gremlin.com/docs/getting-started/compatibility/#caveats)
  - Linux
  - Containers
  - Windows
  - Cloud platforms
- 以下のinjecting failureをGUIのダッシュボードから発生させることができる [Gremlin Docs Attacks](https://www.gremlin.com/docs/fault-injection/attacks/)
- ただし、無料版？だと制限がありそう。。正式版は年間契約のみで年500万～1000万以上だそう。。
  - CPU: Generates high load for one or more CPU cores.
  - Disk: Writes files to disk to fill it to a specific percentage.
  - IO: Puts read/write pressure on I/O devices such as hard disks.
  - Memory: Allocates a specific amount of RAM.
  - ProcessKiller: Kills the specified process, which can be used to simulate application or dependency crashes. Note: Process attacks do not work for Process ID 1, consider a Shutdown attack instead.
  - Shutdown: Performs a shutdown (and an optional reboot) on the host operating system to test how your system behaves when losing one or more cluster machines.
  - TimeTravel: Changes the host's system time, which can be used to simulate adjusting to daylight saving time and other time-related events.
  - Blackhole: Drops all matching network traffic.
  - DNS: Blocks access to DNS servers.
  - Latency: Injects latency into all matching egress network traffic.
  - PacketLoss: Induces packet loss into all matching egress network traffic.

#### 製品比較資料
- [カオスツールの選び方](https://dwp.nttdata.com/document/document-3706ab3013970fe400088cb5b88cf3a8-01UXZOCZ5G2YQM6GGEIJDJVYC52UZ3YZJ2) （※NTTD内リンクのため注意）

#### 使用する製品について
- AWS Fault Injection Simulatorを中心に使用し、場合によってはChaos MeshやLitmusも使用できそうであれば使用してみる。
- Gremlinは年間契約が必要のため使用しない。
- 検証環境を構築する上での考慮点
  - AWS Fault Injection Simulatorを最大限に活用するために、「ECS」、「EKS」、「RDS」は極力利用する。
  - Chaos MeshやLitmusも使用できるように、コンテナは可能な限りKubernetes（EKS）を使用する。
