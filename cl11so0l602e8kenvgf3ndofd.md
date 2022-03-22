## Popup Modal Bottom, Top, Left or Right Modal

If my blogs help you in any way you can show your contribution. 
Pay as you wish
**https://rzp.io/l/dpvitnJCSP**

![popup model botttom cover photo blog.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1647931831789/fJ_dvc_zX.jpg)
In your file add a button from where you want to open the modal. I my case I am using as share option so that when user click on share button the modal will open from bottom.
```
<button onclick="openNav()" class="btn btn-sm btn-outline-primary">Share</button>
```
You can have any element just mention **"onclick function"** on that html element
Now create an another div with class overlay of id myNav. Inside this div you can have any design. I am using share button therefore I will be placing icons like Facebook, twitter etc.
```
<div class="overlay" id="myNav">
                        <div class="row ml-10">
                            <div class="col-12 p-3 font-weight-bold cursor-pointer">
                                <h6 href="javascript:void(0);" style="font-size: 35px;" class="closebtn vertical-align" onclick="closeNav()">
                                    × &nbsp;<span style="font-size: 20px;">Share</span>
                                </h6>
                            </div>

                            <div class="col-2">
                                <a href="" class="rounded-circle"><img width="50px" src="https://img.icons8.com/color/240/000000/facebook-circled--v1.png"/></a>
                            </div>

                            <div class="col-2">
                                <a href="" class="rounded-circle"><img width="50px" src="https://img.icons8.com/fluency/144/000000/twitter-circled.png"/></a>
                            </div>

                            <div class="col-2">
                                <a href="" class="rounded-circle"><img width="50px" src="https://img.icons8.com/color/144/000000/linkedin-circled--v1.png"/></a>
                            </div>

                            <div class="col-2">
                                <a href="" class="rounded-circle"><img width="50px" src="https://img.icons8.com/color/144/000000/whatsapp--v1.png"/></a>
                            </div>

                            <div class="col-2">
                                <a href="" class="rounded-circle"><img width="50px" src="https://img.icons8.com/fluency/96/000000/telegram-app.png"/></a>
                            </div>

                            <div class="col-2">
                                <a href="" class="rounded-circle"><img width="50px" src="https://img.icons8.com/external-tal-revivo-color-tal-revivo/96/000000/external-reddit-gives-you-the-best-of-the-internet-in-one-place-logo-color-tal-revivo.png"/></a>
                            </div>
                        </div>

</div>
```
Add the CSS to the page inside <head> tag.
```
  <style>
       /*Laptop and above screen sizes*/
        @media (min-width: 769px) {
            .overlay {
                width: 50%;
                height: 0;
                position: fixed;
                bottom: 0;
                left: 25%;
                z-index: 1;
                background-color: #add8e6;
                overflow-y: hidden;
                transition: 1.0s;
            }
        }

        @media (max-width: 768px) {
            .overlay {
                width: 100%;
                height: 0;
                position: fixed;
                bottom: 0;
                left: 0;
                z-index: 1;
                background-color: #add8e6;
                overflow-y: hidden;
                transition: 1.0s;
            }
        }

        .overlay a:hover,
        .overlay a:focus {
            color: black;
        }
        .closebtn{
            cursor: pointer;
        }
</style>
```
Add the below script right before the body ends
```
<script>
    function openNav() {
        document.getElementById(
            "myNav").style.height = "30%";
    }

    function closeNav() {
        document.getElementById(
            "myNav").style.height = "0%";
    }

</script>
```
If my blogs help you in any way you can show your contribution. 
Pay as you wish
**https://rzp.io/l/dpvitnJCSP**
