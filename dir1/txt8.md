###### ActionCable, websocket
---


```channels/chat_message_channel.rb
class ChatMessageChannel < ApplicationCable::Channel
  def subscribed;stream_from "chat_message_channel";end

  def unsubscribed
  end
  
  def speak(data)
    @message = Message.create(body: data["message"])
    data['id'] = @message.id
    ActionCable.server.broadcast 'chat_message_channel',message: data
  end
end


```

```message.js
App.chatmessage = App.cable.subscriptions.create("ChatMessageChannel", {
  connected: function() {
  },
  
  disconnected: function() {
  },
  
  received: function(data) {
    let message_id = data['message']['id']
    let message_text = data['message']['message']
    let html =`
      <tr>
      <td><a href="/messages/${message_id}">Show</td>
      <td><a href="/messages/${message_id}/edit">Edit</td>
      <td><a data-confirm="Are your sure?" rel="nofollow" data-method="delete" href="/messages/${message_id}">Destroy</td>
      <td>${message_text}</td>
      </tr>`
        $("#message").append(html)
  },
  
  speak: function(message) {
    return this.perform('speak',{message: message});
  }
  }, $(document).on('keypress', '[data-behavior~=speak_chat_messages]', function(event) {
  if (event.keyCode === 13) {
    App.chatmessage.speak(event.target.value)
      event.target.value = ''
      event.preventDefault()
  }  
}));
```

```.sh
rails g scaffold 
vi app/views/index.html.erb
```

```app/views/index.html.erb
<p id="notice"><%= notice %></p>

<h1></h1>

<table>
  <thead>
    <tr>
      <th colspan="3"></th>
    </tr>
  </thead>
  
  <tbody id="message">
    <% @message.each do |message| %>
      <tr>
        <td><%= link_to 'Show', message %></td>
        <td><%= link_to 'Edit', edit_message_path(message) %></td>
        <td><%= link_to 'Destroy', message, method: :delete, data: {confirm: 'Are you sure?' } %></td>
        <td><%= message.body %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>
<%= link_to 'New Message', new_message_path %>

</div>
<br/>
<form>
  <label>
    websockets SEND: <input type="text" data-behavior="speak_chat_messages">
  </label>
</form>
```


```javascripts/cable.js
(function() {
  this.App || (this.App = {});
  
  App.cable = ActionCable.createConsumer();
  
}).call(this);
```

```message.js
```


```chat_message_channel.rb
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```

