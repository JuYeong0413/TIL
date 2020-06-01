# Django version upgrade 방법  
처음에 [Django docs](https://docs.djangoproject.com/en/3.0/howto/upgrade-version/#installation)에 나와 있는 방법을 이용했다.  
```bash
$ python3 -m pip install -U Django
```
```bash
Successfully installed Django-3.0.6 asgiref-3.2.7 pytz-2020.1 sqlparse-0.3.1
```
라는 문구가 출력되어서 업그레이드가 완료된 줄 알았는데,  
```bash
$ pip3 list
```
명령어로 확인해보니 이전 버전으로 떴다.  
```bash
$ pip3 install django --upgrade
```
StackOverflow를 찾아보다 위 명령어를 시도했고,  
```bash
ERROR: Could not install packages due to an EnvironmentError: [Errno 13] Permission denied: 'RECORD'
Consider using the `--user` option or check the permissions.
```
uninstalling 중에 에러 메시지가 떴다. 아마 RECORD 프로젝트 관련해서 문제가 생긴 듯 했다.  
에러 메시지에 나온대로 `--user` option을 사용해서 다시 시도했다.  
```bash
$ pip3 install django --upgrade --user
```
잘 설치되었고, `pip3 list`에도 최신 버전으로 출력되었다.  
그런데 `zsh: command not found: django-admin` 문제가 생겼다.  
사용하고 있던 virtual environment에 최신 버전의 Django를 설치하니 해결되었다. [StackOverflow 참고자료](https://stackoverflow.com/a/61570750)  
```bash
$ pipenv shell
$ pipenv install django==3.0.6
```
