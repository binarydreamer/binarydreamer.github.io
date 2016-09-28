---
title:  "Active Record Soft Delete"
date:   2015-06-02 19:55:00
---

It's been a little while since I posted. I needed an easy to use concern to allow certain Active Record models the ability to soft delete. I looked over a couple gems but did not want to include yet another gem to my project. I wanted something I had more control over and after a little searching I found some code that intrigued me.

<a href="http://stackoverflow.com/questions/23017070/how-to-hide-records-rather-than-delete-them-soft-delete-from-scratch#answer-23017174" target="_blank">http://stackoverflow.com/questions/23017070/how-to-hide-records-rather-than-delete-them-soft-delete-from-scratch</a>

After looking over the solution I decided to make a Rails 4 concern to fit my needs. Here is what came of it.

app/models/concerns/soft_delete.rb

{% highlight ruby %}
require 'active_support/concern'

module SoftDelete
  extend ActiveSupport::Concern

  def destroyed?
    self.deleted_at.present?
  end

  def deleted?
    destroyed?
  end

  def restore
    self.update(deleted_at: nil)
  end

  included do
    alias_method :force_destroy, :destroy
    alias_method :force_delete,  :delete

    scope :not_deleted,   -> { where(deleted_at: nil) }
    scope :deleted,       -> { where("#{self.table_name}.deleted_at IS NOT NULL") }

    scope :not_destroyed, -> { where(deleted_at: nil) }
    scope :destroyed,     -> { where("#{self.table_name}.deleted_at IS NOT NULL") }

    def destroy
      self.update(deleted_at: DateTime.current)
    end

    def delete
      destroy
    end
  end
end

{% endhighlight %}

Then to include it in one of your models

app/models/foo.rb

{% highlight ruby %}
class Foo < ActiveRecord::Base
  include SoftDelete

  #...
end
{% endhighlight %}

Don't forget to make a migration for the model. We need to add a deleted_at datetime.

{% highlight ruby %}
class AddDeletedAtToFoos < ActiveRecord::Migration
  def change
    add_column :foos, :deleted_at, :datetime, default: nil, index: true
  end
end
{% endhighlight %}

Now you have access to the following:

{% highlight ruby %}
#destroy or delete will soft delete
foo.destroy
foo.delete

# force delete or destroy, these are aliases of the origial methods
foo.force_destroy
foo.force_delete

# check if a model instance was soft deleted
foo.destroyed?
foo.deleted?

# remove the soft delete by the resotre method
foo.restore

# four chainable scopes
Foo.not_deleted
Foo.not_destroyed

Foo.deleted  
Foo.destroyed

  # e.g.
  Foo.not_destroyed.order(:name)

{% endhighlight %}
