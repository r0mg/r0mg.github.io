---
layout: post
title: "Visualizing Schema with Rails-ERD"
date: 2015-11-01 16:44:05 -0500
comments: true
categories: Flatiron School
---
{% img right /images/scheduler_pic.png 250 250 Class Visual %}
Establishing appropriate relationships between objects is fundamental in developing a functional app. As our apps increase in complexity, so does the level of abstraction in describing these relationships (what IS a post-tag, anyway?). A good way to get started is usually to draw it out, as we often do in class.

This is a great first step for visual learners like myself. In fact, I find it so useful in understanding the various "has-many" and "has-many-through" relationships that I almost always start off each lab by physically mapping out the schema and drawing the connecting lines between associated attributes. 

{% img left https://www.dlsweb.rmit.edu.au/lsu/content/1_StudySkills/study_tuts/learning%20styles/graphics/visual.gif A Typical Visual Learner%}
While this method has certainly helped me figure out which associations need to be set in each model, it feels a little...outdated. Also, the more join relationships and connected models involved, the messier the drawing, which kind of defeats the purpose of mapping it out to begin with. Finally, it only represents the relationships that we're trying to establish, and there's no way to check that it matches what's actually been coded.  


Enter Rails-ERD, a lovely Rails plugin that generates entity-relationship diagrams based on Active Record models. It works by using open source graph visualization software called <a href="http://www.graphviz.org/">Graphviz</a> to load the attributes and associations within existing Active Record models and combining them into a single diagram. Once installed, a single command can generate a fancy diagram of any domain. For instance, Rails-ERD yields the following for the school scheduler example:

{% img center /images/scheduler_domain.png Scheduler Diagram %}

To generate pretty diagrams of your own apps, first install Graphviz by entering the following on the command line: 
<div class="console"><pre><code class="console"><span class="gp">// ♡</span> brew install graphviz   
</code></pre>
</div>

Next, add <code>rails-erd</code> to the application's <code>Gemfile</code>:
<div class="highlight"><pre><code class="ruby"><span class="n">group</span> <span class="ss">:development</span> <span class="k">do</span>
  <span class="n">gem</span> <span class="s2">"rails-erd"</span>
<span class="k">end</span>
</code></pre>
</div>

After running <code>bundle install</code>, simply run the following to generate a first diagram:
<div class="console"><pre><code class="console"><span class="gp">// ♡</span> erd
</code></pre>
</div>

If everything worked properly, Rails-ERD generated an <code>erd.pdf</code> file of the diagram, which can be seen by running <code>open erd.pdf</code> from the app directory. That's it!

The default output generated through <code>erd</code> can additionally be customized to specify which attributes should be included in the diagram, the types of relationships to display, inheritance hierarches, etc. For instance, if we wanted to display single table inheritance models, only direct relationships, foreign keys and other fields in the table, we would run <code>erd --inheritance --direct --attributes=foreign_keys,content</code> to get the following:

{% img center /images/scheduler_custom.png Scheduler Diagram %}

A comprehensive list of customization options can be found <a href="http://rails-erd.rubyforge.org/customise.html">here</a>. There's also a Rails-ERD API that can be used to extend and/or customize generated output, but that's for another blog ;)



