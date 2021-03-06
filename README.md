# # Week 4 Comprehensive Assessment

# Fork this repository, update this file to include your answers, and submit a pull request. You may refer to any notes, past projects, or external resources you want. The questions do not have to be completed in order.

# 1. My app has Students who are organized into Houses. What lines should I add to `config/routes.rb` to create a complete set of nested RESTful routes for these resources?

resources :houses do
  resources :students
end


# 2. Write a migration to create a `students` table with columns for name, age, and something to link the student to the `House` they belong to.

1) rails g migration CreateStudents

2) def change
      create_table :students do |t|
        t.string :name
        t.integer :age
        t.belongs_to :house, index:  true
      end
    end

3) rake db:migrate


# 3. Assuming a Student can only be in a single House, what kind of associations should I define on these models? Do I need to introduce any new models, and if so, what associations should I define on those?

class Student < ActiveRecord::Base
  belongs_to :house
end

class House < ActiveRecord::Base
  has_many :students, dependent: :destroy
end

# 4. Now I want Students to be able to enroll in Courses. What kind of associations should I define on these models? Do I need to introduce any new models, and if so, what associations should I define on those?
class Student < ActiveRecord::Base
  belongs_to :house
  has_many :courses, dependent: :destroy
end

class Course < ActiveRecord::Base
  belongs_to :student
end

(need to introduce this new model "Course")

# 5. I want Students to be able to leave Comments on both Courses and Professors. What kind of associations should I define on these models? Do I need to introduce any new models, and if so, what associations should I define on those?
class Student < ActiveRecord::Base
  belongs_to :house
  has_many :courses, dependent: :destroy
  has_many :comments, dependent: :destroy
end

class Course < ActiveRecord::Base
  belongs_to :student
    has_many :comments, as: :commentable, dependent: :destroy
end

class Professor < ActiveRecord::Base
  belongs_to :student
    has_many :comments, as: :commentable, dependent: :destroy
end


# 6. When creating a new Student in my students controller, assuming I have a `student_params` method and the house the student should belong to is stored in `@house`, what code should I write to link the student to the house?
@house.students.find(params[:id])

# 7. If I'm using Devise with a User model, what code should I write in a controller or view to find out whether a user is signed in? And how can I get a reference to the currently-signed-in user?
<% if user_signed_in? %> Logged in as <%= current_user.email %>. <%= link_to 'Edit profile', edit_user_registration_path %> | <%= link_to "Logout", destroy_user_session_path, method: :delete %> <% else %> <%= link_to "Sign up", new_user_registration_path %> | <%= link_to "Login", new_user_session_path %> <% end %>

# 8. If I'm using Devise with a User model, what code should I write in my students controller so that only signed-in users can access the `new` and `create` actions?
  def create
    @variable = current_user.variables.new(variable_params)
etc.

# 9. When I deploy a new version of my app to Heroku that contains a new migration, how can I run that migration on my production database?
rake db:load:schema

# 10. If accessing my app on Heroku gives me an uninformative "something went wrong" error message, how can I find out what the real error is?

?
