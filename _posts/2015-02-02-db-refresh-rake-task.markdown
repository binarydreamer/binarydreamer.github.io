---
title:  "Rails db:refresh Rake Task"
date:   2015-02-02 16:15:00
---

This is a simple rake task I tend to use when I'm toying with my db and I need to start over.

lib/tasks/db.rake

{% highlight ruby %}
namespace :db do
  desc "Refresh db"
  task refresh: :environment do
    Rake::Task["db:drop"].invoke
    Rake::Task["db:create"].invoke
    Rake::Task["db:migrate"].invoke
    Rake::Task["db:seed"].invoke
  end
end
{% endhighlight %}
