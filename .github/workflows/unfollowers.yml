import requests

def get_github_followers(username):
    followers_url = f"https://api.github.com/users/{username}/followers"
    following_url = f"https://api.github.com/users/{username}/following"

    followers_response = requests.get(followers_url)
    following_response = requests.get(following_url)

    if followers_response.status_code == 200 and following_response.status_code == 200:
        followers = []
        following = []

        page = 1
        per_page = 30

        while True:
            followers_url = f"https://api.github.com/users/{username}/followers?page={page}&per_page={per_page}"
            following_url = f"https://api.github.com/users/{username}/following?page={page}&per_page={per_page}"

            followers_response = requests.get(followers_url)
            following_response = requests.get(following_url)

            if followers_response.status_code == 200 and following_response.status_code == 200:
                followers_page = followers_response.json()
                following_page = following_response.json()

                if not followers_page and not following_page:
                    break

                followers.extend(followers_page)
                following.extend(following_page)

                page += 1
            else:
                print("Error: Failed to retrieve GitHub followers or following.")
                return None, None

        followers_list = [follower['login'] for follower in followers]
        following_list = [follow['login'] for follow in following]

        return followers_list, following_list
    else:
        print("Error: Failed to retrieve GitHub followers or following.")
        return None, None

def unfollow_non_followers(username):
    followers, following = get_github_followers(username)

    if followers and following:
        non_followers = [user for user in following if user not in followers]

        # Save unfollowers to a file
        with open("unfollowers.txt", "w") as file:
            file.write("\n".join(non_followers))

        print("Unfollowers list saved to 'unfollowers.txt' file.")
    else:
        print("Unable to unfollow non-followers.")

# Example usage
username = "muhammed-mizaj"
unfollow_non_followers(username)
