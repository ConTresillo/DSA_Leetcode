# WEB PROGRAMMING  
**S Tharun Parykshyt**  
**24BCE1119**

---

## Assignment Summary

The task involved reproducing two given webpage designs using **only HTML and inline styling**.  
No external CSS files or internal `<style>` tags were used.  
The focus was on understanding how basic webpage layouts can be constructed using fundamental HTML positioning techniques.

---

## Page 1 – Website Layout Representation

### What the Page Contains

This page is designed to resemble a complete website structure and includes the following components:

- A **header area** where a logo block and the website title are aligned horizontally  
- A **navigation bar** stretching across the width of the page  
- A **row of three sections**, each represented by a separate colored block  
- A **main content region** that occupies most of the page width  
- A **sidebar section** placed alongside the main content  
- A **footer section** positioned at the bottom  

```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Layout</title>
</head>
<body style="margin:0; padding:0; background-color:#333333; font-family:Arial, Helvetica, sans-serif;">

    <div style="width:800px; margin:10px auto; background-color:#000000; padding:5px; box-sizing:border-box;">

        <div style="width:100%; height:80px; background-color:#008000;">
            <div style="float:left; width:25%; height:80px; background-color:#ff0000; display:flex; align-items:center; justify-content:center; font-weight:bold;">
                LOGO BOX
            </div>
            <div style="float:left; width:75%; height:80px; background-color:#008000; display:flex; align-items:center; justify-content:center; font-size:28px;">
                MY WEBSITE
            </div>
            <div style="clear:both;"></div>
        </div>

        <div style="width:100%; height:50px; background-color:#ffff00; display:flex; align-items:center; justify-content:center; font-size:22px; font-weight:bold;">
            MENU BAR
        </div>

        <div style="width:100%; height:120px;">
            <div style="float:left; width:33.33%; height:120px; background-color:#0000ff; color:#ffffff; display:flex; align-items:center; justify-content:center; font-weight:bold;">
                PART 1
            </div>
            <div style="float:left; width:33.33%; height:120px; background-color:#666666; display:flex; align-items:center; justify-content:center; font-weight:bold;">
                PART 2
            </div>
            <div style="float:left; width:33.33%; height:120px; background-color:#c090c0; display:flex; align-items:center; justify-content:center; font-weight:bold;">
                PART 3
            </div>
            <div style="clear:both;"></div>
        </div>

        <div style="width:100%; height:350px;">
            <div style="float:left; width:75%; height:350px; background-color:#00e5c0; display:flex; align-items:center; justify-content:center; font-size:26px; font-weight:bold;">
                MAIN CONTENT
            </div>
            <div style="float:left; width:25%; height:350px; background-color:#808000; display:flex; align-items:center; justify-content:center; font-size:20px; font-weight:bold; writing-mode:vertical-rl;">
                SIDE AREA
            </div>
            <div style="clear:both;"></div>
        </div>

        <div style="width:100%; height:70px; background-color:#66a3d2; display:flex; align-items:center; justify-content:center; font-size:22px; font-weight:bold;">
            FOOTER
        </div>

    </div>
</body>
</html>

```
### How the Layout Is Achieved

Each section is arranged by floating elements horizontally where required.  
The `float` property is used to place blocks next to each other, while `clear` is applied to prevent overlap between different rows.  
This approach demonstrates how structured layouts were traditionally created before modern layout systems like Flexbox and Grid.

---

## Page 2 – Content-Oriented Webpage

### What the Page Contains

This page follows a simple article-style design:

- A prominent **title bar** at the top of the page  
- A **content area** consisting of an image and descriptive text  
- The image appears on one side, with text aligned beside it  
- A minimal color scheme to keep the focus on readability  

```html
<!DOCTYPE html>
<html>
<head>
    <title>Simple Article Page</title>
</head>
<body style="margin:0; padding:0; background-color:#000000; font-family:Arial, Helvetica, sans-serif;">

    <div style="width:900px; margin:20px auto; background-color:#ffffff; padding-bottom:40px;">

        <div style="width:100%; background-color:#d8f0a8; padding:25px 0; text-align:center;">
            <span style="font-size:36px; letter-spacing:4px; color:#885f77; font-weight:bold;">
                MY WEBPAGE
            </span>
        </div>

        <div style="width:100%; padding:30px 40px 0 40px;">

            <div style="float:left; width:40%; text-align:center;">
                <img src="https://upload.wikimedia.org/wikipedia/commons/3/36/Lemon.png"
                     alt="Image"
                     style="max-width:100%; height:auto;">
            </div>

            <div style="float:left; width:60%; padding-left:30px; color:#555555; font-size:14px; line-height:1.5;">
                <p style="margin-top:0;">
                    This is a sample paragraph written to show text next to an image. The content is just for practice and does not have any special meaning.
                </p>
                <p>
                    This paragraph is added to make the page look like a real article. The main purpose is to understand how image and text alignment works.
                </p>
            </div>

            <div style="clear:both;"></div>
        </div>

    </div>
</body>
</html>

```
### How the Layout Is Achieved

The image is floated to one side, allowing the text content to automatically flow next to it.  
This creates a natural text-wrapping effect commonly seen in articles, blogs, and informational webpages.  
The structure emphasizes content presentation rather than complex layout styling.

---

## Key Takeaways

- Learned to build webpage layouts using only HTML and inline styles  
- Understood practical use of `float` and `clear` for positioning elements  
- Observed how different webpage sections are visually organized  
- Gained insight into foundational layout techniques used in early web design
