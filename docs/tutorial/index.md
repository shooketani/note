# tutorial
- [「クラウドネイティブアプリケーションの基本」](https://news.mynavi.jp/techplus/series/AWS/)の実施結果を取りまとめる。

## AWS ECS上に構築するSpringアプリケーション

### AWS構成図
<iframe src="https://nttdatajpprod-my.sharepoint.com/personal/sho_oketani_jp_nttdata_com/_layouts/15/Doc.aspx?sourcedoc={a3fc9b2e-764e-4926-90ef-85352722e1fb}&amp;action=embedview&amp;wdAr=1.7777777777777777" width="714px" height="432px" frameborder="0">これは、<a target="_blank" href="https://office.com/webapps">Office</a> の機能を利用した、<a target="_blank" href="https://office.com">Microsoft Office</a> の埋め込み型のプレゼンテーションです。</iframe>

### AWSリソース一覧
- [VPC](https://shooketani.github.io/note/tutorial/vpc)
- [target group](https://shooketani.github.io/note/tutorial/tg)
- [subnet](https://shooketani.github.io/note/tutorial/subnet)
- [security group](https://shooketani.github.io/note/tutorial/sg)
- [route table](https://shooketani.github.io/note/tutorial/routetable)
- [nat gateway](https://shooketani.github.io/note/tutorial/natgw)
- [keypair](https://shooketani.github.io/note/tutorial/keypair)
- [internet gateway](https://shooketani.github.io/note/tutorial/igw)
- [Elastic Load Balancing](https://shooketani.github.io/note/tutorial/elb)
- [Elastic IP](https://shooketani.github.io/note/tutorial/eip)
- [ECS cluster](https://shooketani.github.io/note/tutorial/ecs)

### アプリケーション
- 作成したJavaソースは[GitHub](https://github.com/shooketani/ecs-sample)上にコミット
- 上記のJavaソースはDocker上で動作するようになっており、Docker ImageをDockeHub上にコミット
  - [shooketani/ecs-sample-backend-for-frontend](https://hub.docker.com/repository/docker/shooketani/ecs-sample-backend-for-frontend)
  - [shooketani/ecs-sample-backend](https://hub.docker.com/repository/docker/shooketani/ecs-sample-backend)　
- アプリケーションは以下のような画面遷移をする
  - 「 Call backend service」を押下すると、ecs-sample-backendで起動しているAPIへリクエストし、レスポンス結果を画面表示する

![image](https://user-images.githubusercontent.com/116000206/207867749-0e3f5536-793c-450f-af16-957cb08e4572.png)
![image](https://user-images.githubusercontent.com/116000206/207867764-9d7be5f0-5fe1-459f-944a-faf1b8a6cec3.png)

- ecs-sample-backendのAPIへ直接リクエストした結果は以下の通り。

```
C:\Users\Sho>curl localhost:8080/backend/api/v1/users
[{"userId":"1","userName":"Sho"},{"userId":"2","userName":"Ken"}]
C:\Users\Sho>
```


### AWS上での起動
- publicとprivateのECSで、それぞれ2インスタンスずつ起動した際のAWSコンソールは以下の通り。

![image](https://user-images.githubusercontent.com/116000206/208639699-5c755bdf-9d66-41be-afbc-159affb62bf6.png)
![image](https://user-images.githubusercontent.com/116000206/208640068-cba36dce-c5f7-49c5-9c54-566ebec4b466.png)
![image](https://user-images.githubusercontent.com/116000206/208640191-52651968-51e5-43bb-9e1e-70da83b70ebc.png)

- ブラウザからも問題なく接続できたことを確認。
  - [http://ma-oketanisu-alb-public-1498014606.ap-northeast-1.elb.amazonaws.com/backend-for-frontend/index.html](http://ma-oketanisu-alb-public-1498014606.ap-northeast-1.elb.amazonaws.com/backend-for-frontend/index.html)
  
![image](https://user-images.githubusercontent.com/116000206/208640388-d4275c15-899c-4840-a811-50d932c80195.png)
![image](https://user-images.githubusercontent.com/116000206/208640415-59fc6dd8-ed65-4249-b244-6f0860ec6963.png)



