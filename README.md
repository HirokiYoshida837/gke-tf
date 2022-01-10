# gke-terraform

## about

Provisioning GKE Cluster with [HashiCorp Terraform Tutorial documents](https://learn.hashicorp.com/tutorials/terraform/gke)

このチュートリアルでは、2node/AZ構成 でのGKEクラスタが生成されます。(asia-northeast1-a,b,cでそれぞれ2つずつ生成されるので、6nodeのクラスタになる)

適当に、region/azを変更したり、nodeの数を調整したりしてください。



GKEでは、standard利用でのzone clusterの場合には、クラスタの管理料金が無料になるらしい。
無料枠を使いたい場合は、これも活用してください。なお、一度クラスタを作成した後、region clusterに変更することはできません。

> 1 請求先アカウントあたり 1 つの Autopilot クラスタまたは 1 つのゾーンクラスタを無料でご利用いただけます。

> クラスタを作成した後は、それをゾーンクラスタからリージョン クラスタに、またはリージョン クラスタからゾーンクラスタに変更することはできません。


## using

以下を実行して、GKEクラスタを作成。5~10分ほどかかるので、気長に待ちます。

※ `~/.config/gcloud/application_default_credentials.json` が自動で参照されて、このアカウントの権限で実行されるので注意してください。 

```shell
$ terraform apply
```

kubectlで操作できるようにcredentialを設定
```shell
$ gcloud container clusters get-credentials $(terraform output -raw kubernetes_cluster_name) --region $(terraform output -raw region)
```

kubectlから情報を取得してみる
```shell
$ kubectl get nodes

NAME                                                  STATUS   ROLES    AGE     VERSION
gke-k8s-benkyo-gke-k8s-benkyo-gke-nod-29f4ba0a-4xjh   Ready    <none>   8m23s   v1.21.5-gke.1302
gke-k8s-benkyo-gke-k8s-benkyo-gke-nod-29f4ba0a-kfj6   Ready    <none>   8m22s   v1.21.5-gke.1302
gke-k8s-benkyo-gke-k8s-benkyo-gke-nod-7e2f8633-7dl5   Ready    <none>   8m29s   v1.21.5-gke.1302
gke-k8s-benkyo-gke-k8s-benkyo-gke-nod-7e2f8633-931x   Ready    <none>   8m29s   v1.21.5-gke.1302
gke-k8s-benkyo-gke-k8s-benkyo-gke-nod-d2e5177f-g2t7   Ready    <none>   8m23s   v1.21.5-gke.1302
gke-k8s-benkyo-gke-k8s-benkyo-gke-nod-d2e5177f-hnm4   Ready    <none>   8m23s   v1.21.5-gke.1302
```

おわり。


## refs

- hashicorp - Terraform
  - [hashicorp/learn-terraform-provision-gke-cluster](https://github.com/hashicorp/learn-terraform-provision-gke-cluster)
  - [Using GKE with Terraform | Guides | hashicorp/google | Terraform Registry](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/using_gke_with_terraform)**
  - [Provision a GKE Cluster (Google Cloud) | Terraform - HashiCorp Learn](https://learn.hashicorp.com/tutorials/terraform/gke)
  - [google_container_cluster | Resources | hashicorp/google | Terraform Registry](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/container_cluster)


- Terraform module
  - [terraform-google-modules/terraform-google-kubernetes-engine: A Terraform module for configuring GKE clusters.](https://github.com/terraform-google-modules/terraform-google-kubernetes-engine)

- other
  - [Terraformを使ってGKEクラスタを作成する。](https://www.runsystem.net/ja/magazine/iot/terraform-o-tsukatte-gke-kurasuta-o-sakusei-suru/)


- GCP document/blog
  - [Terraform による迅速なクラウド基盤の構築とワークロードのデプロイ | Google Cloud Blog](https://cloud.google.com/blog/ja/products/gcp/using-the-cloud-foundation-toolkit-with-terraform)
