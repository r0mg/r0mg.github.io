---
layout: post
title: "Sending Timed Emails through ActiveJob"
date: 2015-12-08 18:36:08 -0500
comments: true
categories: 
---
I've learned a lot about sending email through Rails during this project mode. Since our app (eBae) is an auction site with varying bidding windows and auction end times, our team thought it would be neat to go a step beyond a generic email notification to alert interested bidders when the end times for their watched items was soon approaching.

To do this, we made a new "watchlist" mailer to send email notifications when items are added or removed from the watchlist, and a reminder for when the end time for an item on the watchlist is one hour away. The challenge in setting up the reminder notification is the time variance. Where our other mailers were instantiated through a create action (such as a new user account, or new sale), the reminder email would need to have a background process running to monitor the remaining time.

Enter ActiveJob. Per the RailsGuides, "Active Job is a framework for declaring jobs and making them run on a variety of queuing backends."
When a "Job" is created, the task it is to perform is placed in a queue for a designmated amount of time. These queues are maintained by built-in adapters such as Sidekiq, Resque, and Delayed Job. For the purposes of sending reminder e-mails, Delayed Job does the trick.

The first step in sending a reminder email is to generate a new Job:
<div class="console"><pre><code class="console"><span class="gp">// ♡</span> rails g job reminder_email   
</code></pre>
</div>
This step generates <code>reminder_email_job.rb</code> file in the <code>app/jobs directory.</code>

Within this file, the <code>perform</code> is defined as follows:
<div class="console"><pre><code class="html"><span class="gp">
class ReminderEmailJob < ActiveJob::Base
  queue_as :default

  def perform(watchlist)
    @watchlist = watchlist
    WatchlistMailer.item_ending(@watchlist).deliver_later
  end
end
</code></pre>
</div>
The same argument <code>(watchlist)</code> that would normally be passed directly into the mailer from the controller is instead passed into the <code>ReminderEmailJob</code>, which is then passed into the regular mailer by calling on the appropriate method. In this case, the method in the <code>WatchlistMailer</code> to send out the reminder is <code>item_ending</code>.

The next step is to modify the controller action. Prior to implementing the job, the mailer action in the create method would probably look something like this:
<div class="console"><pre><code class="html"><span class="gp">
WatchlistMailer.item_ending(watchlist).deliver_now
</code></pre>
</div>
With the Job in place, it changes to the following:
<div class="console"><pre><code class="html"><span class="gp">
EndingEmailJob.set(wait_until: watchlist.alert_time).perform_later(watchlist)
</span></code></pre>
</div>
...which directs the mailer action to the Job. Note that the <code>wait_until: watchlist.alert_time</code> is what determines the length of the delay. In this case, an <code>alert_time</code> method was defined in the  model to be one hour before a watched listing's ending time. Time parameters for delayed jobs are highly customizable.

The last step is to configure the Delayed Jobs backend adapter for the backend process.

1. Add <code>gem 'delayed_job_active_record'</code> to the Gemfile
2. Run the following from the terminal: 
<code class="console"><span class="gp">
// ♡ bundle
// ♡  rails generate delayed_job:active_record
// ♡  rake db:migrate
</span></code>
3. Add <code>config.active_job.queue_adapter = :delayed_job</code> to the <code>/config/environments/production.rb</code> and <code>/config/environments/development.rb</code> files.
4. Run <code>// ♡ bundle exec rake jobs:work</code> alongside <code>rails s</code> to run the jobs in queue. While running the localhost, this must be running in the backend to ensure that the delayed jobs are processed.
5. Enjoy!

