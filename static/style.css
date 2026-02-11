import json
import os
from flask import Flask, render_template, request, redirect, url_for


app = Flask(__name__)

def get_json_path():
    base_dir = os.path.dirname(os.path.abspath(__file__))
    return os.path.join(base_dir, "posts.json")


def load_posts():
    base_dir = os.path.dirname(os.path.abspath(__file__))
    json_path = os.path.join(base_dir, "posts.json")
    with open(json_path, "r", encoding="utf-8") as f:
        return json.load(f)

def fetch_post_by_id(post_id):
    posts = load_posts()
    for post in posts:
        if post.get("id") == post_id:
            return post
    return None

def save_posts(posts):
    with open(get_json_path(), "w", encoding="utf-8") as f:
        json.dump(posts, f, indent=4)


@app.route("/")
def index():
    blog_posts = load_posts()
    return render_template("index.html", posts=blog_posts)


@app.route("/add", methods=["GET", "POST"])
def add():
    if request.method == "POST":
        posts = load_posts()
        if posts:
            max_id = max(post["id"] for post in posts)
            new_id = max_id + 1
        else:
            new_id = 1
        title = request.form.get("title")
        author = request.form.get("author")
        content = request.form.get("content")
        new_post = {
            "id": new_id,
            "title": title,
            "author": author,
            "content": content,
        }
        posts.append(new_post)
        save_posts(posts)
        return redirect(url_for("index"))
    return render_template("add.html")


@app.route("/delete/<int:post_id>")
def delete(post_id):
    posts = load_posts()
    posts = [post for post in posts if post.get("id") != post_id]
    save_posts(posts)
    return redirect(url_for("index"))


@app.route("/update/<int:post_id>", methods=["GET", "POST"])
def update(post_id):
    posts = load_posts()
    post = None
    for p in posts:
        if p.get("id") == post_id:
            post = p
            break
    if post is None:
        return "Post not found",
    if request.method == "POST":
        post["title"] = request.form.get("title")
        post["author"] = request.form.get("author")
        post["content"] = request.form.get("content")

        save_posts(posts)
        return redirect(url_for("index"))
    return render_template("update.html", post=post)


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
