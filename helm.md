## 3 больших концепта
1. *Чарт* - это пакет в helm. Он содержит все определения ресурсов, необходимые для запуска приложения, инструмента или сервиса в кластере Kubernetes. Считайте его эквивалентом Kubernetes формуле Homebrew, Apt dpkg или RPM-файлу Yum.
2. *Репозиторий* - это место, где чарты могут быть собраны и совместно использованы.
3. *Релиз* - это экземпляр графика, запущенный в кластере Kubernetes. Один график может быть установлен много раз в одном и том же кластере. И при каждой установке создается новый релиз. Рассмотрим диаграмму MySQL. Если вы хотите, чтобы в вашем кластере работали две базы данных, вы можете установить этот график дважды. Каждая из них будет иметь свой собственный релиз, который, в свою очередь, будет иметь свое собственное имя релиза.

Helm устанавливает чарты в кубер, создает новые релизы каждую инсталляцию, а для того, чтобы найти новый чарт, нужно искать в репозиториях.

## `helm search` Нахождение чартов

Helm поставляется с мощной командой поиска. Она может использоваться для поиска двух различных типов источников:

`helm search hub ищет` в [Artifact Hub](https://artifacthub.io/), где перечислены схемы helm из десятков различных репозиториев.
helm search repo ищет в репозиториях, которые вы добавили в локальный клиент helm (с помощью `helm repo add`). Этот поиск выполняется по локальным данным, и подключение к публичной сети не требуется.
Вы можете найти общедоступные графики, запустив helm search hub:

```
$ helm search hub wordpress
URL                                                 CHART VERSION APP VERSION DESCRIPTION
https://hub.helm.sh/charts/bitnami/wordpress        7.6.7         5.2.4       Web publishing platform for building blogs and ...
https://hub.helm.sh/charts/presslabs/wordpress-...  v0.6.3        v0.6.3      Presslabs WordPress Operator Helm Chart
https://hub.helm.sh/charts/presslabs/wordpress-...  v0.7.1        v0.7.1      A Helm chart for deploying a WordPress site on ...

```
Приведенный выше поиск выполняет поиск всех таблиц wordpress на Artifact Hub.
При отсутствии фильтра рулевой поискового узла показывает все доступные диаграммы.
Используя `helm search repo`, вы можете найти названия графиков в репозиториях, которые вы уже добавили:

```
$ helm repo add brigade https://brigadecore.github.io/charts
"brigade" has been added to your repositories
$ helm search repo brigade
NAME                          CHART VERSION APP VERSION DESCRIPTION
brigade/brigade               1.3.2         v1.2.1      Brigade provides event-driven scripting of Kube...
brigade/brigade-github-app    0.4.1         v0.2.1      The Brigade GitHub App, an advanced gateway for...
brigade/brigade-github-oauth  0.2.0         v0.20.0     The legacy OAuth GitHub Gateway for Brigade
brigade/brigade-k8s-gateway   0.1.0                     A Helm chart for Kubernetes
brigade/brigade-project       1.0.0         v1.0.0      Create a Brigade project
brigade/kashti                0.4.0         v0.4.0      A Helm chart for Kubernetes
```

## `helm install`  Установить пакет

Чтобы установить новый пакет, используйте команду helm install. В самом простом виде она принимает два аргумента: Имя релиза, которое вы выбираете сами, и имя чарта, который вы хотите установить.

```
$ helm install happy-panda bitnami/wordpress
NAME: happy-panda
LAST DEPLOYED: Tue Jan 26 10:27:17 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
** Please be patient while the chart is being deployed **

Your WordPress site can be accessed through the following DNS name from within your cluster:

    happy-panda-wordpress.default.svc.cluster.local (port 80)

To access your WordPress site from outside the cluster follow the steps below:

1. Get the WordPress URL by running these commands:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace default -w happy-panda-wordpress'

   export SERVICE_IP=$(kubectl get svc --namespace default happy-panda-wordpress --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
   echo "WordPress URL: http://$SERVICE_IP/"
   echo "WordPress Admin URL: http://$SERVICE_IP/admin"

2. Open a browser and access WordPress using the obtained URL.

3. Login with the following credentials below to see your blog:

  echo Username: user
  echo Password: $(kubectl get secret --namespace default happy-panda-wordpress -o jsonpath="{.data.wordpress-password}" | base64 --decode)
```

