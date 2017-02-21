1.請解釋 database.yml, routes.rb, 和 Gemifle 分別是什麼？ 他們分別在一個 Rails 專案裡的什麼位置？ 他們為什麼對一個 Rails 專案如此重要？
```
  database.yml是資料庫設定檔，其所在路徑為config/database.yml
  routes.rb能夠設定url所對應的的contorller動作，根據routes.rb就能夠快速了解rails app有哪些功能，路徑是/config/routes.rb
  Gemfile中會有rails app會用到的gem列表，如果再其中增加新的gem
  ，使用terminal在app根目錄輸入bundle指令就會把所有gem都安裝完畢，路徑是/Gemfile
```

2.MVC 架構裡的 M, V, 和 C 分別代表什麼？
```
  M = model
  V = view
  C = controller
```

3.請解釋 CRUD 是哪四個字的縮寫？
```
  C = create
  R = read
  U = update
  D = delete
```

4.請問在 routes.rb 裡面加入以下程式碼會產生出哪一些 url？ 
```ruby
		resources :users
```
(提示：在 browser 輸入http://localhost:3000/rails/info/routes)
  
```ruby

  get '/users', to: 'users#index'
  post '/users', to: 'users#create'
  get '/users/new', to: 'users#new'
  get '/users/:id/edit', to: 'users#edit'
  get '/users/:id', to: 'users#show'
  patch 'users/:id', to: 'users#update'
  put 'users/:id', to: 'users#update'
  delete 'users/:id' to: 'users#delete'

```

5.請解釋 model 檔案和 migration 檔案的差別
```
  migration可以使用ruby的語法去建立table，和增減欄位
  model可以設定不同table之間的關聯性，設定table每個欄位的限制。
```

6.若今天發現一個 migration 檔寫錯，請問我應該用什麼指令回復到上一個版本的 migration?
```
  rake db:rollback
```

7.假設今天
	* 我要在資料庫裡產出一個叫 group 的資料表
	* 裡面包括的欄位名稱和相對應的資料型別是： 
		**name (string)**,
		**description (text)**,
		**members (integer)**
    * 請寫出一個能產生出以上需求的 migration 檔
```ruby
class GroupsTable < ActiveRecord::Migration[5.0]
  def change

    create_table :groups do |t|
      t.string :name
      t.text :description
      t.integer :members

      t.timestamps null: false
    end

  end
end

```

8.請解釋什麼是 ActiveRecord? 
```
ActiveRecor能夠自動把TSQL翻譯成ruby語法，用ruby語法快速進行資料庫的操作
```


9.若今天需要為 ```Project``` 和 ```Issue``` 這兩個 Model 建立一對多的關係，請寫出實作上所需要的 migratiion 和 model 檔案 
```ruby
migratiion :

class ProjectsAndIssuesTable < ActiveRecord::Migration[5.0]
  def change

    create_table :projects do |t|
      t.string :name
      
      t.timestamps null: false
    end

    create_table :issues do |t|
      t.integer :project_id
      
      t.timestamps null: false
    end

  end
end

model :

class Project < ActiveRecord::Base
  has_many :issues

end

class Issue < ActiveRecord::Base
  belongs_to :project

end

```

10.若今天我有以下 model 檔：

  ```ruby
  class User < ActiveRecord::Base
    has_many :groups_users
    has_many :groups, through: :groups_users 
  end
  ```

  請寫出和這個 model 檔相關連的 model 檔，以及這些 model 檔所需要的資料庫欄位
```ruby
  class Group < ActiveRecord::Base
    has_many :groups_users
    has_many :users, through: :groups_users
  end

  class GroupUser < ActiveRecord::Base
    belongs_to :group
    belongs_to :user
  end

```

11.延續第10題，如果需要讓一個叫 "Bob" 的使用者產生一個名字叫做 "Rails is Fun" 的社團，應該如何在 rails console 裡實作出來？
```
  假設 Bob 的 user_id = 1，Rails is Fun 的 group_id = 1

  Group.create(name:"Rails is Fun")
  GroupUser.create(group_id:1,user_id:1)
```

12.延續第11題，請寫一段程式碼確保使用者在建立新社團時社團名不可以是空白，而且不能超過50個字
```ruby
  class Group < ActiveRecord::Base
    has_many :groups_users
    has_many :users, through: :groups_users

    validates :name, uniqueness: true
    validates :name, length: { maximum: 50 }
  end

```