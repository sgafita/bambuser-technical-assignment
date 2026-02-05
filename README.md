# Technical assignment

The goal of this assignment is to complete as many tasks as possible listed below in the TODO list.

## Scenario

A developer has built a landing page for a Live Video Shopping event, but there are still bugs to solve and improvements to make before the page can be used in production. We need you to update the code and fix the tasks listed in the TODO list further down on this page so everything is working as expected once launched.

## Expectation

Here is the specification and high-level requirements for this landing page.

On this landing page, we are going to promote a Live Video Shopping show.

The landing page consists of:

- **A countdown:** A graphical countdown. We should be able to set the date and time of the event. The countdown should disappear when done.
- **A header:** This header should have the value of "We go live in" before the countdown is done. After the target time is passed, the value of the header should be "We are live!".
- **A CTA element:** A card including an image and a button. The whole card should be clickable and trigger the Bambuser Player with a recorded test show.

## Test the current behavior

### 1. Run the project

You are able to run the current project in either way below:

- In the terminal, run `yarn start` within the project working directory
- Or, open the `src/index.html` in a browser

### 2. Modify the target time

In order to test the countdown on different occasions, in the `src/index.html`, you need to update the `data-date` and `data-time` to a closer time (e.g. current time + 1 min). So you can see how the countdown behaves when it reaches the target time.

## TODO

- [x] Make the countdown loop stop once it reaches the target time.
      **Status:** Fixed  
       **Explanation:** Modified `src/countdown.js`.
  - **Issue:**  
    The condition was `distance == 0`, which is unreliable because `requestAnimationFrame` might skip the exact millisecond where the distance equals zero, causing the countdown to continue into negative values.

  - **Fix:**  
    Changed the condition to `if (distance <= 0)` to ensure the countdown stops correctly once the target time is reached.

  - **Code:**
    ```js
    if (distance <= 0) {
      select(config.target + " .day .num").innerHTML = addZero(0);
      config.callback();
      return;
    }
    ```

- [x] Make the countdown element disappear after the target time is reached.
      **Status:** Fixed  
       **Explanation:** Handled in the callback function in `src/index.html`.
  - **Fix:**  
    The countdown element is explicitly hidden once the countdown finishes.

  - **Code:**
    ```js
    select(".countdown").style.display = "none";
    ```

- [x] The CTA element should show up when the countdown is done.
      **Status:** Fixed  
       **Explanation:** Also handled in the callback function in `src/index.html`.
  - **Fix:**  
    The CTA element is displayed when the countdown completes.

  - **Code:**
    ```js
    select(".cta").style.display = "flex";
    ```

- [x] The default font for the landing page is set to `Neue Haas Grotesk Display`, but the font is not applied. Investigate why and provide a solution.
      **Status:** Fixed  
       **Explanation:** Modified `src/assets/css/style.css`.
  - **Issue:**  
    The font import path was incorrect relative to the CSS file location.

  - **Fix:**  
    Updated the import statement to use the correct relative path.

  - **Code:**
    ```css
    @import "../fonts/fonts.css";
    ```

- [x] Center the `wrapper` element by modifying the CSS. The `wrapper` element is not fully centered (it is horizontally centered but not vertically).
      **Status:** Fixed  
       **Explanation:** Modified `src/assets/css/style.css`.
  - **Issue:**  
    The `body` element was defined as a flex container but missing vertical alignment.

  - **Fix:**  
    Added vertical centering to the flex container.

  - **Code:**
    ```css
    body {
      display: flex;
      align-items: center;
    }
    ```

- [x] The header text must be changed to "We Are Live!" when the countdown is done.
      **Status:** Fixed  
       **Explanation:** Updated `src/index.html`.
  - **Fix:**  
    Updated the header text inside the countdown callback function.

  - **Code:**
    ```js
    select(".wrapper > h1").innerHTML = "We Are Live!";
    ```

## Questions

Once you have fixed the CTA button, you should be able to click it. On click, the Bambuser One-to-many player will be opened. When clicking a product in the player, you will see an error page. We need you to explain as much as possible below:

- What is the error?
- Why does it happen?
- Potential solution(s)?

See how to reproduce the error: [Video](producing-iframe-error.mp4)

### Answer

1. **What is the error?**  
   The error shown is a browser security restriction preventing the content from being displayed inside an `<iframe>`. It typically appears as a "refused to connect" message or an error indicating that framing is not allowed.

2. **Why does it happen?**  
   This occurs because the server hosting the embedded content sends security headers such as `X-Frame-Options: SAMEORIGIN` or `X-Frame-Options: DENY`, or defines `Content-Security-Policy` rules with `frame-ancestors`. These headers explicitly prevent the page from being embedded on external domains.

3. **Potential solution(s):**
   - **When you do not own the content:**  
     This restriction cannot be bypassed. The correct approach is to open the content in a new browser tab (`target="_blank"`) or in a separate window instead of embedding it in an iframe.
   - **When you own the content:**  
     The server configuration can be adjusted to allow embedding from specific domains:
     - Remove or relax the `X-Frame-Options` header.
     - Use `Content-Security-Policy` with a `frame-ancestors` directive that explicitly allows the landing page domain.

## How to deliver the assignment

- You will receive the assignment in a `.zip` file that you can unzip and start working on it.
- Make sure you read the README.md carefully before you start working on the tasks.
- _Would be nice_ to use Git and commit your changes. You can use your local Git repository or a private remote repository.
- When you are done, make a zip archive of your project, including the .git directory _in case you chose to use Git_.
- There is no right or wrong, so, feel free to be creative while delivering the expectations.
- If you have any questions, reach out via email.
