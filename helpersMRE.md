# Helpers MRE

Guide to use existing functions in PMPGO for field validation, etc.

# Date Format in JS

Follow the steps below to create date type inputs.

```bash
#HTML
<input type="text" name="dateNameInput" class="form-control txt-date-picker text-right"/>
```
```bash
#JQUERY
$(document).ready(function() {
    #init date picker
    initDatePicker();
});

function initDatePicker() {
            $('.txt-date-picker').datepicker({
                changeMonth: true,
                changeYear: true,
                yearRange: '2019:+10',
                orientation: "auto",
                todayHighlight: true,
                todayBtn: "linked",
                autoclose: true,
                clearBtn: true,
                dateFormat: 'd-M-y',
                beforeShow: function(input, inst) {
                    setDatepickerPos(input, inst)
                }
            });
        }
```

# Use the following code when you need to format dynamic dates

```bash
#JQUERY
#include this plugin
<script src="{{ asset('js_system/gantt/dhtmlxgantt.js?v=7.1.3')}}"></script>

#include this const
const format_date_english = gantt.date.date_to_str("%d-%M-%y");

#use this function wherever you want
function makeDate(){
      today = new Date();
      var dd = String(today.getDate()).padStart(2, '0');
      var mm = String(today.getMonth() + 1).padStart(2, '0');            
      var yyyy = today.getFullYear();
      var today = yyyy + '-' + mm + '-' + dd;
      dateFormat = today;
      dateFormat = format_date_english(new Date(dateFormat + 'T00:00:00'))
}
```

# In blade views use the EnglishDateFormat helper to display date

```bash
#BLADE
<input type="text" name="nameInput" class="form-control text-right" value="{{EnglishDateFormat($obj->valueDate)}}" />

#PHP
EnglishDateFormat($valueDate);

```
# Avoid copying pasting into inputs

```bash
#JQUERY
$(document).ready(function() {
    $('.name-class').on("cut copy paste", function(e) {
            e.preventDefault();
    });
});
```
# Prevent the user from typing letters and characters in numeric and date fields

```bash
#add this in inputs
onkeypress="return (event.charCode >= 48 && event.charCode <= 57)"

#example
<input type="text" class="form-control" onkeypress="return (event.charCode >= 48 && event.charCode <= 57)" />
```

# Configure basic table

```bash
#html
<div class="table-responsive">
<table id="tblWhatever" class="table table-bordered table-hover"></table>
</div>

#JQUERY
$(document).ready(function() {
    initDatatables();
});

function initDatatables(){
_tableNameTable = $("#tblWhatever").DataTable({
                "dom": '<"row"<"text-left col-2"f>>lt<"bottom"i><"clear">',
                "searching": false,
                "bPaginate": false,
                "ordering": false,
            });
}
```
# Other helpers that could be useful

```php
#returns data formatted in currency type
if (! function_exists('CurrencyFormat')) {
  function CurrencyFormat ($amount){
    $amount = $amount == '' ? 0 : $amount;
    if($amount < 0) {
      $amount *= -1;
      return '-$' . number_format($amount, 2);
    } else {
      return '$' . number_format($amount, 2);
    }
  }
}
#use like this
$dataFormat = CurrencyFormat($dataToFormat);
#----------------------------------------------------------------------------
#Remove currency formatting to be stored in the database
 if (! function_exists('removeCurrencyFormat')) {
    function removeCurrencyFormat($currency){
      $value = str_replace('$', '' ,$currency);
      $value = str_replace(',', '' ,$value);
      return trim($value);
    }
  }
#use like this
$dataFormat = removeCurrencyFormat($dataToFormat);
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
