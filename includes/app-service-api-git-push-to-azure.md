Azure CLI를 사용하여 API 앱에 대한 원격 배포 URL을 가져온 다음 원격으로 푸시할 수 있도록 로컬 Git 배포를 구성합니다.

```bash
giturl=$(az webapp deployment source config-local-git -n $app_name \ -g myResourceGroup --query [url] -o tsv)

git remote add azure $giturl
```

Azure 원격에 푸시하여 앱을 배포합니다. 앞서 배포 사용자를 만들 때 만든 암호를 묻는 메시지가 나타납니다. Azure Portal에 로그인할 때 사용하는 암호가 아닌, 빠른 시작에서 이전에 만든 암호를 입력해야 합니다.

```bash
git push azure master
```
