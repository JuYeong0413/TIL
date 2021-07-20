# Database seeding

`db/seeds/`에 여러 파일로 나누어서 seeding이 가능하다.
```
db/
├───migrate/
├───seeds/
        001_elements.rb
        002_benefits.rb
        003_notes.rb
```

#### db/seeds/001_elements.rb
```ruby
def seed_elements
  puts 'Seeding elements...'
  
  Element.find_or_create_by!(
    name: 'juyeong',
    // ...
  )
  
  puts 'Seeding elements done.'
end
```
위와 같은 방식으로 메소드 안에 코드를 작성하고,  

#### db/seeds.rb
```ruby
require_relative './seeds/001_elements'

puts 'Seeding...'
seed_elements # 001_elements.rb 파일에 작성한 메소드
puts 'Seeding done.'
```
`seeds.rb` 파일에서 호출해주면 된다.  
이후 `rails db:seed` 명령어를 terminal에서 실행하면 끝  
