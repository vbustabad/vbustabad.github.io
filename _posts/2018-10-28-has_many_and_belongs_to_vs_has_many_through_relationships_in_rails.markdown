---
layout: post
title:      "Has_and_Belongs_to_Many vs. Has_Many :Through Relationships in Rails"
date:       2018-10-28 12:04:06 -0400
permalink:  has_many_and_belongs_to_vs_has_many_through_relationships_in_rails
---

Working on the Rails Portfolio Project led me to discover the many-to-many relationships available for Active Record models, including has_and_belongs_to_many and has_many :through. This blog post will discuss the differences between the two many-to-many relationships in Rails and consider the circumstances where one might utilize one relationship over the other.

A has_and_belongs_to_many relationship can be utilized where there is a direct relationship between the Active Record models. For instance, an author can have many books. Books can also belong to many authors. In this instance, there is a direct relationship between books and authors. Therefore, there is no need for a 3rd Active Record model. However, if there is an indirect relationship between the Active Record models or there is a need for a 3rd entity, the has_many :through association should be utilized. As an example, doctors have many patients through appointments. Patients can also belong to many doctors through appointments. The relationship between doctors and patients exists through appointments. There is a need to establish a 3rd Active Record model, which in this case represents appointments, because there would be no connection between doctors and patients if appointments did not exist.

```
class Author < ActiveRecord::Base
  has_and_belongs_to_many :books
end
 
class Book < ActiveRecord::Base
  has_and_belongs_to_many :authors
end
```

```
class Doctor < ActiveRecord::Base
  has_many :appointments
  has_many :patients, through: :appointments
end

class Patient < ActiveRecord::Base
  has_many :appointments
  has_many :doctors, through: :appointments
end

class Appointment < ActiveRecord::Base
  belongs_to :doctor
  belongs_to :patient
end
```

When evaluating whether to use a has_and_belongs_to_many or a has_many :through association, it is also important to consider the attributes or information that needs to be captured regarding the models. If there is an attribute that is unique to a 3rd Active Record model that we need to record or keep track of for our application, then a has_many :through association would be preferable. For instance, using the doctors and patients example, we would like to keep track of the dates and times for the appointments. Note that date and time are characteristics that are unique to appointments. We would therefore need to create a 3rd Active Record model, appointments, in order to keep track of the information. If, however, there is a simpler association between the Active Record models, then the has_and_belongs_to_many association would be preferable. 

Lastly, it is also important to consider that a has_many :through association offers greater flexibility than the has_and_belongs_to_many association. As mentioned in the Rails Guides for Active Record Associations, a has_many :through association should be used “if you need validations, callbacks or extra attributes on the join model”.  If, from the beginning of the development process for the application, it is uncertain whether the many-to-many connection between the Active Record models should be extended or if there will be additional information that needs to be captured, then the has_many :through relationship would be a better choice. Note that both the has_and_belongs_to_many and the has_many :through relationships require a join table. A join table would be represented by appointments in the doctors and patients example. In the example of the Adopt-a-Dog application that I built for the Rails Portfolio Project, the join table would be represented by adoption comments which are the comments that are associated to a particular adoption of a dog and which are submitted as feedback for the adoption process. However, when evaluating whether to use a has_and_belongs_to_many association or a has_many :through association, consideration must be given to the various points that have been discussed in this blog post. This will ensure that the best decision will be made according to the particular needs of your application. 

```
class CreateDoctors < ActiveRecord::Migration
  def change
    create_table :doctors do |t|
      t.string :name
      t.string :department

      t.timestamps null: false
    end
  end
end

class CreatePatients < ActiveRecord::Migration
  def change
    create_table :patients do |t|
      t.string :name
      t.integer :age

      t.timestamps null: false
    end
  end
end

class CreateAppointments < ActiveRecord::Migration
  def change
    create_table :appointments do |t|
      t.datetime :appointment_datetime

      t.timestamps null: false
    end
  end
end

```

```
class CreateAdoptions < ActiveRecord::Migration[5.2]
  def change
    create_table :adoptions do |t|
      t.references :owner, foreign_key: true
      t.references :dog, foreign_key: true
    end
  end
end

class CreateComments < ActiveRecord::Migration[5.2]
  def change
    create_table :comments do |t|
      t.text :feedback
    end
  end
end

class CreateAdoptionsComments < ActiveRecord::Migration[5.2]
  def change
    create_table :adoptions_comments do |t|
      t.references :adoption, foreign_key: true
      t.references :comment, foreign_key: true
    end
  end
end
```
