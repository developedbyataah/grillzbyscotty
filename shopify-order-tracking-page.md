# Adding an Order Tracking page to Shopify
These are the instructions for updating a [Shopify](shopify.com) page to include order tracking
* This custom feature is free to use in comparison to subscription services like [AfterShip](aftership.com)
* This is a script to create a page on Shopify to enable customers to track their orders
* An alternate version of this document, created with Google Docs and using images throughout the process is available [here](https://docs.google.com/document/d/1RSvthxvw8RPDdG_dNpwngNFe_ZaiUCxc-5Oz5fObt40/edit?usp=sharing)
## Overview of steps:
1. Create page
2. Add script
3. Add page to navigation
4. Update confirmation e-mail
## Detailed steps:
1. Click `Online Store` in the left bar
2. Click `Pages`
3. Click `Add page`
4. Title the page "Order Tracking”
5. Click `Save`
6. Under `Content` click `Show HTML`
7. Copy the **Tracking Page script** from below and paste it in the `Content` box
### TRACKING PAGE SCRIPT:
```
<div style="text-align: center;">Enter your tracking number here to see your order status.</div>
<br />
<div style="text-align: center;"><input type="text" id="YQNum" maxlength="50" /> <input type="button" value="TRACK" onclick="doTrack()" /></div>
<div id="YQContainer"></div>
<!--Script code can be put in the bottom of the page, wait until the page is loaded then execute.-->
<script type="text/javascript" src="//www.17track.net/externalcall.js"></script>
<script type="text/javascript">// <![CDATA[
function doTrack() {
    var num = document.getElementById("YQNum").value;
    if(num===""){
        alert("Enter your number."); 
        return;
    }
    YQV5.trackSingle({
        //Required, Specify the container ID of the carrier content.
        YQ_ContainerId:"YQContainer",
        //Optional, specify tracking result height, max height 800px, default is 560px.
        YQ_Height:560,
        //Optional, select carrier, default to auto identify.
        YQ_Fc:"0",
        //Optional, specify UI language, default language is automatically detected based on the browser settings.
        YQ_Lang:"en",
        //Required, specify the number needed to be tracked.
        YQ_Num:num
    });
}
// ]]></script>
```
8. Click `Save`
9. Click `View page` to preview the page
> You should now see:
>>    1. A title for the page (“Order Tracking”)
>>    2. Text saying “enter your tracking number here to see your order status”
>>    3. A text field to enter tracking numbers
>>    4. A “track” button to submit the tracking number
10. Copy the URL (link) for the order tracking page from the address bar
11. Paste the link somewhere to save it for later
12. Click `Navigation` on the left bar
13. Click `Main menu`
14. Click `Add menu item`
15. Click into the `Link` text field and click `Pages`
16. Click the `Order Tracking` page (which we just created)
17. Click `Add`
18. Click `Save menu`
> The Order Tracking navigation link should now be in the navigation menu
>> You can preview this by clicking back on `Online Store` in the left bar and then clicking on the `View your store` buttom in the upper right side of the webpage
19. Click `Settings` in the left bar
20. Click `Notifications`
21. Scroll to the `Shipping` section and click on `Shipping confirmation`
22. Click `Edit code` to edit the HTML code
> **NOTE:** It’s easiest to delete the entire code and replace it with the “Replace Confirmation Script”
### REPLACE CONFIRMATION E-MAIL SCRIPT:
```
{% if fulfillment.item_count == item_count %} 
  {% capture email_title %}Your order is on the way{% endcapture %}
  {% capture email_body %}<p>Good news, we have just shipped out your order 🙂 You can copy your tracking number below and enter it in the order tracking page to view your delivery status.<br><br>
Tracking number: {{fulfillment.tracking_numbers}}<br><br>
If your tracking number isn't working, please wait 2-3 days and try again. In most cases, your order is on the way, but the system information is delayed.</p>
{% endcapture %}
{% elsif fulfillment.item_count > 1 %} 
  {% if fulfillment_status == 'fulfilled' %}
    {% capture email_title %}The last items in your order are on the way{% endcapture %}
    {% capture email_body %}The last items in your order are on the way. Track your shipment to see the delivery status.{% endcapture %}
  {% else %}
    {% capture email_title %}Some items in your order are on the way{% endcapture %}
    {% capture email_body %}Some items in your order are on the way. Track your shipment to see the delivery status.{% endcapture %}
  {% endif %}
{% else %} 
  {% if fulfillment_status == 'fulfilled' %}
    {% capture email_title %}The last item in your order is on the way{% endcapture %}
    {% capture email_body %}The last item in your order is on the way. Track your shipment to see the delivery status.{% endcapture %}
  {% else %}
    {% capture email_title %}One item in your order is on the way{% endcapture %}
    {% capture email_body %}One item in your order is on the way. Track your shipment to see the delivery status.{% endcapture %}
  {% endif %}
{% endif %}
{% capture email_emphasis %}Estimated delivery date: <strong>{{fulfillment.estimated_delivery_at | date: "%B %-d, %Y"}}</strong>{% endcapture %}
<!DOCTYPE html>
<html lang="en">
  <head>
  <title>{{ email_title }}</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <meta name="viewport" content="width=device-width">
  <link rel="stylesheet" type="text/css" href="/assets/notifications/styles.css">
  <style>
    .button__cell { background: {{ shop.email_accent_color }}; }
    a, a:hover, a:active, a:visited { color: {{ shop.email_accent_color }}; }
  </style>
</head>
  <body>
    <table class="body">
      <tr>
        <td>
          <table class="header row">
  <tr>
    <td class="header__cell">
      <center>
        <table class="container">
          <tr>
            <td>
              <table class="row">
                <tr>
                  <td class="shop-name__cell">
                    {%- if shop.email_logo_url %}
                      <img src="{{shop.email_logo_url}}" alt="{{ shop.name }}" width="{{ shop.email_logo_width }}">
                    {%- else %}
                      <h1 class="shop-name__text">
                        <a href="{{shop.url}}">{{ shop.name }}</a>
                      </h1>
                    {%- endif %}
                  </td>
                    <td class="order-number__cell">
                      <span class="order-number__text">
                        Order {{ order_name }}
                      </span>
                    </td>
                </tr>
              </table>
            </td>
          </tr>
        </table>
      </center>
    </td>
  </tr>
</table>
          <table class="row content">
  <tr>
    <td class="content__cell">
      <center>
        <table class="container">
          <tr>
            <td>
              
            <h2>{{ email_title }}</h2>
            <p>{{ email_body }}</p>
            {% if fulfillment.estimated_delivery_at %}
              <p>{{ email_emphasis }}</p>
            {% endif %}
            {% if order_status_url %}
              <table class="row actions">
  <tr>
    <td class="actions__cell">
      <table class="button main-action-cell">
        <tr>
          <td class="button__cell"><a href="https://yourstorename.com/pages/track" class="button__text">Track your order</a></td>
        </tr>
      </table>
      {% if shop.url %}
    <table class="link secondary-action-cell">
      <tr>
        <td class="link__cell">or <a href="{{ shop.url }}">Visit our store</a></td>
      </tr>
    </table>
{% endif %}
    </td>
  </tr>
</table>
            {% else %}
              {% if shop.url %}
    <table class="row actions">
      <tr>
        <td class="actions__cell">
          <table class="button main-action-cell">
            <tr>
              <td class="button__cell"><a href="{{ shop.url }}" class="button__text">Visit our store</a></td>
            </tr>
          </table>
        </td>
      </tr>
    </table>
{% endif %}
            {% endif %}
            </td>
          </tr>
        </table>
      </center>
    </td>
  </tr>
</table>
          <table class="row section">
  <tr>
    <td class="section__cell">
      <center>
        <table class="container">
          <tr>
            <td>
              <h3>Items in this shipment</h3>
            </td>
          </tr>
        </table>
        <table class="container">
          <tr>
            <td>
                          
<table class="row">
  {% for line in fulfillment.fulfillment_line_items %}
  <tr class="order-list__item">
    <td class="order-list__item__cell">
      <table>
        <td>
          {% if line.line_item.image %}
            <img src="{{ line.line_item | img_url: 'compact_cropped' }}" align="left" width="60" height="60" class="order-list__product-image"/>
          {% endif %}
        </td>
        <td class="order-list__product-description-cell">
          {% if line.line_item.product.title %}
            {% assign line_title = line.line_item.product.title %}
          {% else %}
            {% assign line_title = line.line_item.title %}
          {% endif %}
          {% if line.quantity < line.line_item.quantity %}
            {% capture line_display %} {{ line.quantity }} of {{ line.line_item.quantity }} {% endcapture %}
          {% else %}
            {% assign line_display = line.line_item.quantity  %}
          {% endif %}
          <span class="order-list__item-title">{{ line_title }} × {{ line_display }}</span><br/>
          {% if line.line_item.variant.title != 'Default Title' %}
            <span class="order-list__item-variant">{{ line.line_item.variant.title }}</span><br/>
          {% endif %}
          {% if line.line_item.refunded_quantity > 0 %}
            <span class="order-list__item-refunded">Refunded</span>
          {% endif %}
          {% if line.line_item.discount_allocations %}
            {% for discount_allocation in line.line_item.discount_allocations %}
              {% if discount_allocation.discount_application.target_selection != 'all' %}
                <span class="order-list__item-discount-allocation">
                  <img src="{{ 'notifications/discounttag.png' | shopify_asset_url }}" width="18" height="18" class="discount-tag-icon" />
                  <span>
                    {{ discount_allocation.discount_application.title | upcase }}
                    (-{{ discount_allocation.amount | money }})
                  </span>
                </span>
              {% endif %}
            {% endfor %}
          {% endif %}
        </td>
      </table>
    </td>
  </tr>{% endfor %}
</table>
            </td>
          </tr>
        </table>
      </center>
    </td>
  </tr>
</table>
          <table class="row footer">
  <tr>
    <td class="footer__cell">
      <center>
        <table class="container">
          <tr>
            <td>
              
              <p class="disclaimer__subtext">If you have any questions, reply to this email or contact us at <a href="mailto:{{ shop.email }}">{{ shop.email }}</a></p>
            </td>
          </tr>
        </table>
      </center>
    </td>
  </tr>
</table>
<img src="{{ 'notifications/spacer.png' | shopify_asset_url }}" class="spacer" height="1" />
        </td>
      </tr>
    </table>
  </body>
</html>
```
> **NOTE:** The update that needs to take place in the script is **a change to the highlighted URL**
>> I’m not able to see what the Shopify URL would be from www.grillzbyscotty.com so I need someone who currently has access to the Shopify platform to make these edits, unless I can be added as an admin to the site, then I can go in and make the changes myself. If not, I believe these instructions should be clear enough to follow.
23. Update the highlighted URL in the code with the URL that was copied earlier from the `Order Tracking` page
24. Paste the new code in the `Email body (HTML)` field
25. Click on `Save`
26. Click on `Preview`
27. Now the order preview should appear on the page and it should list order items and information.
28. The process for this task is now complete and fully implemented into Shopify

> **NOTE:** Customers can also send an email to their desired recipient of this order confirmation information

> **ALSO NOTE:** This script does not need to be updated often, fields like `Shop Email address` are automatically fed to the script from Shopify’s platform, so even if you copied this code and process and used it on a different store, it would work exactly the same without any additional changes other than updating the URL in the script.

---

This guide was written by [Ataah Azah](https://github.com/developedbyataah)
