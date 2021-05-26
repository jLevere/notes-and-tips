## Customising the start page of FireFox >= 88 on windows 10

Only a few steps.  We are going to use userChrome/userContent.css to add a simple background image to the browser.

First, we are going to add a new directory on the path `C:\Users\{{user}}\AppData\Roaming\Mozilla\Firefox\Profiles\{{release}}\ ` and name it `chrome`.

In the `chrome` directory add another folder named `img`.  

We will add a css file in the main `chrome` directory named `userContent.css`, and the image for the background in the `img` folder.

As an example, I am using this `userContent.css`
```CSS
@-moz-document url(about:home), url(about:newtab), url(about:privatebrowsing) {
    .click-target-container *, .top-sites-list * {
        color: #fff !important ;
        text-shadow: 2px 2px 2px #222 !important ;
    }

    body::before {
        content: "" ;
        z-index: -1 ;
        position: fixed ;
        top: 0 ;
        left: 0 ;
        background: #f9a no-repeat url(img/16077922455698.jpg) center ;
        background-size: cover ;
        width: 100vw ;
        height: 100vh ;
    }
}
```

After this is done we need to go into firefox, and into the `about:config` section.  Just type `about:config` and it will open.  `about:about` will give you more info about what about commands can do.

Search for `toolkit.legacyUserProfileCustomizations.stylesheets` and change it to true.

Restart your browser and it should work!  
