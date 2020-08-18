###### comments create destory rails6
---

```sh
rails g model Comment content:string image:references
rails g controller comments create destroy
```

```models/comment.rb
belongs_to :image
validates :content, presence: true
```

```models/image.rb
has_many :comments, dependent: destroy
```

```config/routes.rb
resources :images do
  resources :comments, only: [:create, :destroy]
end
```

```app/controller/controllers_comments.rb
private 
def content_params
  params.require(:comment).permit(:content)
end

def create
  iamge = Image.find(params[:image_id])
  @comment = image.comments.build(comment_params)
  if @comment.save
    flash[:success] = "DONE COMMENTS"
    redirect_back(fallback_location image_rl(image.id))
  else
    flash[:danger] = "COULDN'T COMMENTS"
    redirect_back(fallback_location: image_url(image.id))
  end
end

def destroy
  image = Image.find(params[:image_id])
  @comment = image.comments.find(params[:id])
  @comment.destory
  redrect_back(fallback_location: image_path(image))
end

def show
  @image = Image.find(params[:id])
  @comment = @image.comments.build
end
```

```app/views/_form.html.erb
<%= form_with(model: [@image, @comment], local: true) do |f| %>
  <%= f.text_field :content %>
  <%= f.submit "COMMENT", class: "btn btn-primary" %>
<% end %>

<% link_to "DELETE", image_comment_path(image_id: @image, id: comment.id), method: :delete %>
```
---
###### 



```
```

```
```

```
```




