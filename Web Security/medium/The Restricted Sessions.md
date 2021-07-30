# [The Restricted Sessions](https://cybertalents.com/challenges/web/the-restricted-sessions) Writeup

---

## Prerequisites

* Cookies
* Sessions
* GET & POST methods

---

## learn

* Session hijacking attack

---

## Tools

* Burp Suite OR any proxy

---

## Solution

### Recon

1. after we open the challenge we got that page

    ![flag:null](..\Images\The_Restricted_Sessions_1.png)

2. so we open page source to see if there are any     thing interesting, and we got that code

    ```JS
      if(document.cookie !== ''){
        $.post('getcurrentuserinfo.php',{
          'PHPSESSID':document.cookie.match(/PHPSESSID=([^;]+)/)[1]
        },function(data){
          cu = data;
        });
      }
    ```

---

### Exploit

1. we open our proxy and set our scope to the challenge

    ![set scope](..\Images\The_Restricted_Sessions_2.png)

2. we get the first request and sent it to repeater to start our exploit

    ![request](..\Images\The_Restricted_Sessions_3.png)

3. from our simple recon we notice that the web app login is via session id using the default name for session cookie in php  **PHPSESSID** so we add that cookie with any value for testing and see what will happen

    ![The Restricted Sessions](..\Images\The_Restricted_Sessions_4.png)

4. after we adding the cookie in the request the response tell us a directory where the app store the valid sessions id using browser we got them [Sessions id](http://34.77.37.110/restricted-sessions/data/session_store.txt)

    ```text
    iuqwhe23eh23kej2hd2u3h2k23
    11l3ztdo96ritoitf9fr092ru3
    ksjdlaskjd23ljd2lkjdkasdlk
    ```

5. take any ID from them as a valid one and repeat step 3 we got that respond with a new message tell us that there is another cookie named **UserInfo**

    ![The Restricted Sessions](..\Images\The_Restricted_Sessions_5.png)
So we added it to see what will happen
    ![The Restricted Sessions](..\Images\The_Restricted_Sessions_6.png)
we got another message tell us the validation failed as expected

6. from our recon step 2 we got URI for page had UserInfo but we should sent POST request to that page with correct session id to get the right user info for that session

    ![The Restricted Sessions](..\Images\The_Restricted_Sessions_7.png)
after that we go **null** !!!
from your knowledge about POST request we should put a body parameter so we added our session id to the body

    ![The Restricted Sessions](..\Images\The_Restricted_Sessions_8.png)

1. we got the UserInfo from the app so we change the **UserInfo** cookie in our main page request to the name of the user, change the method to GET and see what will happen

    ![The Restricted Sessions](..\Images\The_Restricted_Sessions_9.png)
and finally we got the flag

> sessionareawesomebutifitsecure
