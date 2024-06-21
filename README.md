<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Title</title>

    <script type="text/javascript">
            /*function to validate the entered input by the user*/
            function checkNumber(inpNum) {

            let inpVal = document.getElementById(inpNum).value;
            let newVal = String(toFixedDecimals(inpVal,2));

            if(inpVal.includes("e") || inpVal.includes("E")){
            alert("'e' or 'E' in the entered value are not allowed");
            $('#' + inpNum).val('0');
            $('#' + inpNum).trigger('input');
            $('#'+inpNum).focus();
            return false;
            }

            /*to replace the entered value with the new calculated value, it is written to remove the zeroes entered
            * before the value which serve no purpose.*/
            $('#' + inpNum).val(newVal);

            /*to check if the value contains any spaces*/
            if (newVal.match(/\s/g)) {
            alert("Space is not allowed");
            $('#' + inpNum).val('0');
            $('#' + inpNum).trigger('input');
            $('#'+inpNum).focus();
            return false;
            }
            /*to check if the value is a number or not*/
            if (isNaN(newVal)) {
            alert("Please enter Numeric Value only");
            $('#' + inpNum).val('0');
            $('#' + inpNum).trigger('input');
            $('#'+inpNum).focus();
            return false;
            }
            /*regex expression for checking the first value to be  '-' then after to allow any 15 digits after
            * then having the '.' to separate the decimal digits then allowing only to decimal digits
            * the or condition is the same without the negative sign at the start.*/

            let c = /^(?:\-?\d{1,15}\.\d{1,2}|\-?\d{1,15})$/;
            if (c.test(newVal) || newVal === "") {
            let idot = newVal.indexOf('.');
            if (idot === -1) {
            if (newVal === "") {
            $('#' + inpNum).val(newVal.concat("0.00"));
            } else {
            $('#' + inpNum).val(newVal.concat(".00"));
            }
            } else if (idot !== -1 && newVal.charAt(idot + 2) === "") {
            $('#' + inpNum).val(newVal.concat("0"));
            }
            } else {
            alert("Amount is not allowed more than 15 digits and two decimal places.");
            $('#' + inpNum).val('0');
            $('#' + inpNum).trigger('input');
            $('#'+inpNum).focus();
            return false;
            }
            return true;
            }

    </script>
</head>
<body>
<div class="wrapper">
    <div class="header header-filter" style="background-image: url('assets/img/bg2.jpeg');">
        <div class="container">
            <div class="row tim-row">
                <div class="col-md-8 col-md-offset-2">
                    <div class="brand">
                        <h3 style="color: white">FR Return</h3> <%--@v1014064--%>
                    </div>
                </div>
            </div>

        </div>
    </div>
    <div class="main main-raised"
         style="outline: #1c2529; outline-width: inherit; outline-style: auto; height: inherit">
        <div class="section">
            <div class="container"> <%--@v1014064--%>
                <div class="row" style="vertical-align: inherit;"> <%--@v1014064--%>
                    <div class="col-xs-1">
                        <button class="btn btn-primary btn-fab btn-fab-mini btn-round" onclick="history.back()">
                            <i class="material-icons">arrow_left</i>
                        </button>
                    </div>
                    <div class="col-xs-11">
                        <h3>{{TxtCtrl.heading}}</h3>
                    </div>
                </div>
                <div class="row" style="height: 40px;">
                    <div class="col-md-6" style="background: #b9def0; height: 40px; text-align: center; vertical-align: middle">
                        <h4>Particulars</h4>
                    </div>
                    <div class="col-md-6"
                         style="background: #b9def0; height: 40px; text-align: center; vertical-align: middle">
                        <h4>Current Year</h4>
                    </div>
                </div>
                <div class="row" ng-repeat="row in  TxtCtrl.showList track by $index">
                    <div class="col-md-6"
                         style="text-align: left; vertical-align: middle; height: 40px">
                        <h4>{{row.key}}</h4>
                    </div>
                    <div class="col-md-6">
                        
                        <h4><textarea class="form-control" rows="" cols="90" ng-model="row.value" style="width: 100%;"
                                      ng-hide="TxtCtrl.rmID == '40020301' || TxtCtrl.rmID == '40022705'"
                                      maxlength="4000" ng-blur="TxtCtrl.saveVal(row.value, row.key);"></textarea></h4>
                        
                        <input type="text" class="form-control" style="text-align:right"
                               ng-model="row.value"
                               ng-show="TxtCtrl.rmID == '40020301' || TxtCtrl.rmID == '40022705'" id="val1"
                               onblur="checkNumber('val1');" maxlength="19"
                               ng-blur="TxtCtrl.saveValNo(row.value, row.key);">

                    </div>
                </div>
                <div class="row" ng-show="TxtCtrl.rmID == '40022705'">
                    <p><b>Note:</b> <br>
                        Provisions towards Standard Assets need not be netted from gross advances but shown separately as 'Provisions<br>
                        against Standard Assets', under 'Other Liabilities and Provisions - Others' in Schedule No. 5 of the balance sheet.</p>
                </div>
                <div style="text-align: center">
                    <button class="btn btn-success" ng-click="TxtCtrl.submitIndividual()">
                        Complete
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
<div class="example-modal">
    <div class="modal fade" id="modalSubmit">
        <div class="modal-dialog">
            <div class="modal-header bg-success">
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span></button>
                <div class="modal-title"><b>Success !</b></div>
            </div>
            <div class="modal-content">
                <div class="modal-body" id="popup1">
                    <b>{{TxtCtrl.displayMessage}}</b>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-round btn-success" data-dismiss="modal" ng-click="TxtCtrl.redirectToFRReturn()" >Continue</button>
                </div>
            </div>
        </div>
    </div>
</div>

</body>
</html>
