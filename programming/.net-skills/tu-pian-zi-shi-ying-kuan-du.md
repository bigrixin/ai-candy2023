# 图片自适应宽度

```
<link href="~/Scripts/jquery-ui.css" rel="stylesheet" />
<script src="~/Scripts/jquery-ui.js"></script>
<script src="~/Scripts/jquery-2.1.4.js"></script>
<style>
    #resizable {
        width: 130px;
        height: 170px;
        padding: 0.5em;
        text-align: center;
        display: table-cell;
        vertical-align: middle;
    }

        #resizable h3 {
            text-align: center;
            margin: 0;
        }

    #myimg {
        max-width: 100%;
        max-height: 100%;
        vertical-align: middle;
        white-space: normal !important;
    }
</style>

...........


            <td>
                @Html.DisplayFor(modelItem => item.LotNum)
            </td>
            <td>

                @{ var itemImage_1 = "../../Images/Auctions/" + @ViewBag.PictureDirectory + "/" + @item.LotNum + "_1.jpg";}
                <img src=@itemImage_1 alt="x" id="myimg" class="img-holder" />
            </td>
```
