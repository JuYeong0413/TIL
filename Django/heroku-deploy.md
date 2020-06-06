# Django 프로젝트 Heroku에 배포하기  

## 배포 전 사전 작업  
### 1. [Heroku](https://www.heroku.com/) 회원가입 및 로그인  
### 2. [Heroku CLI 설치](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)  
### 3. `.gitignore` 파일 생성하기  
[gitignore.io](https://www.toptal.com/developers/gitignore) 이용하면 편하다.  
`manage.py` 파일이 있는 디렉토리에 생성해야 함  
#### `.gitignore` 파일 적용하기  
```bash
git rm -r --cached .
git add .
git commit -m "add gitignore"
git push origin master
```
### 4. Procfile 작성  
`manage.py` 파일이 있는 디렉토리에 `Procfile`이라는 이름의 파일을 만든다. (확장자 없음)  
```
web: gunicorn 프로젝트명.wsgi --log-file -
```
프로젝트명은 `django-admin startproject` 할 때 적었던 프로젝트명  
### 5. Gunicorn 설치
```bash
pip install gunicorn
```
### 6. Database 설정  
`Heroku`의 database는 `PostgreSQL`를 기본 옵션으로 하고 있어서 설정해줘야 함  
#### pip 패키지 설치  
```bash
pip install dj-database-url
pip install psycopg2-binary
```
#### `settings.py` 설정 추가  
```python
# Heroku: Update database configuration from $DATABASE_URL.
import dj_database_url
db_from_env = dj_database_url.config(conn_max_age=500)
DATABASES['default'].update(db_from_env)
```
### 7. static file 설정  
#### pip 패키지 설치  
```bash
pip install whitenoise
```
#### `settings.py` 설정 추가  
`MIDDLEWARE` 부분에서 `SecurityMiddleware` 위에 `'whitenoise.middleware.WhiteNoiseMiddleware',`추가  
```python
MIDDLEWARE = [
    'whitenoise.middleware.WhiteNoiseMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```
### 8. 파이썬 관련 라이브러리 설치  
```bash
pip freeze > requirements.txt
```
`pip` 명령어로 설치했던 각종 패키지들을 `Heroku` 서버에도 설치해야 하기 때문에 관련된 목록을 리스트업 해 주는 역할  
### 9. `Python` 버전 작성
`manage.py` 파일이 있는 디렉토리에 `runtime.txt` 파일 생성 후 아래 내용 작성  
```
python-3.8.3
```
### 10. `settings.py` 설정  
```python
SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', '[프로젝트의 SECRET_KEY]')
DEBUG = bool( os.environ.get('DJANGO_DEBUG', True) )
```
### 11. `Github` repository의 `master` branch에 모든 변경사항 `push`하기  

## Heroku에 배포하기  
### 1. Heroku 로그인  
```bash
$ heroku login
```
### 2. Heroku app 생성  
```bash
$ heroku create
```
### 3. Heroku에 push  
```bash
$ git push heroku master
```
### 4. database migration  
```bash
$ heroku run python manage.py migrate
```
`makemigrations`는 `Heroku`에서 자동으로 해줌  
### 5. 배포된 홈페이지 확인  
```bash
$ heroku open
```
### 6. `superuser` 생성  
```bash
$ heroku run python manage.py createsuperuser
```
superuser 사용자 이름, 이메일 주소, 비밀번호, 비밀번호 확인까지 입력하면 끝  
