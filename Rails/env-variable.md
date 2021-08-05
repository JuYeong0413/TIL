# Setting ENV variable manually
1. `config/env_variables.rb`에 설정
```ruby
ENV['DB_NAME'] = ''
ENV['DB_USERNAME'] = ''
ENV['DB_PASSWORD'] = ''
```
2. `config/environment.rb`에서 불러오기 (load와 initialize 사이에 아래 코드 추가)
```ruby
# Load config/env_variables.rb
env_vars = File.join(Rails.root, 'config', 'env_variables.rb')
load(env_vars) if File.exist?(env_vars)
```
3. `.gitignore`에 `config/env_variables.rb`파일 추가
