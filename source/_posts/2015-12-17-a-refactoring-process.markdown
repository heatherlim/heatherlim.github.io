---
layout: post
title: "A Refactoring Process"
date: 2015-12-17 09:29:21 -0500
comments: true
categories: 
---

For my final project, my group members and I were creating an app that would track prices of a selected item over time. Shoppers could add their items to a closet and "follow" other fellow shoppers. So there was a little bit of a social media aspect to it. This was tracked by an activity feed. Initially, I was just concerned with getting it working and because of that, my view really realllyyyy suffered. 

Check out all this logic in my view:
```
<% @user.activity_feed.each do |activity| %>

          <br><br>
          <strong>
            <% if current_user.username == activity.user.username %>
                You
            <% else %>
                <%= link_to activity.user.username, activity.user %>
            <% end %>
          </strong>
          <br>
          <% if activity.trackable_type == "Closet" && activity.action == "create" %>
              <em class="text-muted">created closet </em> <%= activity.trackable_name %><br>
              <%= link_to closet_like_path(user_id: current_user.id, id: Closet.find_by(id: activity.trackable_id)), method: :put do %>
                  Like:
                  <%= activity.user.closets.find_by(name: activity.trackable_name).get_likes.size %>
              <% end %>

          <% elsif activity.trackable_type == "Closet" && activity.action == "destroy" %>
              <em class="text-muted">deleted closet </em> <%= activity.trackable_name %>


          <% elsif activity.trackable.class == Item && activity.action == "create" %>
              <em class="text-muted">Added</em> <%= activity.trackable_name %>
              <em class="text-muted">to</em> <%= activity.trackable_source %><br>
              <%= link_to like_item_path(Item.find_by(id: activity.trackable_id)), method: :put do %>
                  Like:
                  <%= activity.user.items.find_by(name: activity.trackable_name).get_likes.size %>
              <% end %>

          <% elsif activity.trackable.class == Item && activity.action == "destroy" %>

              <em class="text-muted">Deleted</em> <%= activity.trackable_name %>
              <em class="text-muted">from</em> <%= activity.trackable_source %>

          <% elsif activity.trackable.class == User && activity.action == "create" %>
              <em class="text-muted">followed</em> <%= activity.trackable.username %>

          <% elsif activity.trackable.class == User && activity.action == "destroy" %>
              <em class="text-muted">unfollowed</em> <%= activity.trackable.username %>
          <% end %>

      <% end %>

```
and... congratz. You made it to the end. It's kinda gross and awful. So let's think about how to fix that. First off, the activity feed was living in the boards index. I decided to move it over to a partial so the boards index.html.erb page now would just show this:

```
 <%= render partial: 'boards/activity_feed', locals: {user: @user}%>
```
But that still doesn't solve all that logic in the view! So right now I'm thinking that I require a view object of some sort to handle the logic... 

One issue that cropped up is the styling. When a user adds an item to a closet, they should get a message that would kinda look like this:

<em>added</em> mediocre rubber pants <em>to</em> whatever closet

How was I supposed to selectively add styling to a whole message?

So at first I thought oh great all I have to do is put this in the view object:
```
class ActivityFeed

  def self.message(activity)
   if activity.trackable_type == "Closet" && activity.action == "create" 
      "<em class = 'text-muted'>created closet</em> #{activity.trackable_name}" 

      elsif activity.trackable_type == "Closet" && activity.action == "destroy"
      "<em class = 'text-muted'>deleted closet</em> #{activity.trackable_name}" 


      elsif activity.trackable_type == "Item" && activity.action == "create"
      "<em class = 'text-muted'>added</em> #{activity.trackable_name} <em class = 'text-muted'>to</em> #{activity.trackable_source}" 
      
      elsif activity.trackable_type == "Item" && activity.action == "destroy"
      "<em class = 'text-muted'>deleted</em> #{activity.trackable_name} <em class = 'text-muted'>from</em> #{activity.trackable_source}"

      elsif activity.trackable_type == "User" && activity.action == "create"
      "<em class = 'text-muted'>followed</em> #{activity.trackable.username}" 
  
      elsif activity.trackable_type == "User" && activity.action == "destroy"
      "<em class = 'text-muted'>unfollowed</em> #{activity.trackable.username}"
      
  end
end

end
```
and then all I have to do is call html_safe on the partial

```
<%@user.activity_feed.each do |activity|%>
    <br><br>
    <strong>
    <% if current_user.username == activity.user.username %>
    You
    <%else%>
   <%= link_to activity.user.username, activity.user %>
   <% end %>
  </strong>
   <br> 
    <%=ActivityFeed.message(activity).html_safe%>
<%end%>

```
But.... wait up, we don't want to have any html in our view object. It's kinda icky. One of the points of the view object is to separate the html from the logic. 

But the styling was giving me a lot of issues. How could I make sure that only some text remained muted when I was getting back an entire string?

The solution for me was to add more methods into my view object. And that's ok.

```
class ActivityFeed

  attr_accessor :activity

  def initialize(activity)
    @activity = activity
  end

  def actionmessage
    if activity.trackable_type == "Closet" && activity.action == "create"
      "created closet"

      elsif activity.trackable_type == "Closet" && activity.action == "destroy"
      "deleted closet" 

      elsif activity.trackable_type == "Item" && activity.action == "create"
      "added" 
      
      elsif activity.trackable_type == "Item" && activity.action == "destroy"
      "deleted"

      elsif activity.trackable_type == "User" && activity.action == "create"
      "followed" 
  
      elsif activity.trackable_type == "User" && activity.action == "destroy"
      "unfollowed" 
    end
  end

  def subject
    activity.trackable_name
  end

  def preposition
    if activity.action == "create"
      "to"
    else
      "from"
    end
  end

  def closet
    activity.trackable_source
  end

end
```
I also added an attribute accessor to lessen the repetition in the partial when I found myself re-typing ActivityFeed.trackable_type one too many times. So this is what the partial looks like now:

```
<%@user.activity_feed.each do |activity|%>
<% activityview = ActivityFeed.new(activity) %>
    <br><br>
    <strong>
    <% if current_user.username == activity.user.username %>
    You
    <%else%>
   <%= link_to activity.user.username, activity.user %>
   <% end %>
  </strong>
   <br> 
    <% if activity.trackable_type == "Item" %>

      <em class = 'text-muted'><%=activityview.actionmessage%></em> <%=activityview.subject%> <em class = 'text-muted'> <%= activityview.preposition %> </em><%=activityview.closet%>
    <% else %>
      <em class = 'text-muted'> <%=activityview.actionmessage%></em> <%=activityview.subject%>
    <% end %>

<%end%>
```
It's already looking a lot better, half the size it was before and way more readable.

Refactoring is definitely a process for me and it involves many little edits along the way.