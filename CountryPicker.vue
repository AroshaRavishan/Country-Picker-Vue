<script setup>
$(document).ready(function() {
    var telInput = $("#phone");
    // initialise plugin
    telInput.intlTelInput({
        allowExtensions: true,
        formatOnDisplay: true,
        autoFormat: true,
        autoHideDialCode: true,
        autoPlaceholder: true,
        defaultCountry: "auto",
        ipinfoToken: "yolo",

        nationalMode: false,
        numberType: "MOBILE",
        preferredCountries: ["sa", "ae", "qa", "om", "bh", "kw", "ma"],
        preventInvalidNumbers: true,
        separateDialCode: true,
        initialCountry: "auto",
        geoIpLookup: function(callback) {
            $.get("https://ipinfo.io", function() {}, "jsonp").always(function(resp) {
                var countryCode = resp && resp.country ? resp.country : "";
                callback(countryCode);
            });
        },
        utilsScript: "https://cdnjs.cloudflare.com/ajax/libs/intl-tel-input/11.0.9/js/utils.js"
    });

    var reset = function() {
        telInput.removeClass("error");
        form.dial_code = "+" + $("#phone").intlTelInput("getSelectedCountryData").dialCode;
        if ($.trim(telInput.val())) {
            if (telInput.intlTelInput("isValidNumber")) {
                $("#basic_detail").attr('disabled', false).css('opacity', 1);
            } else {
                telInput.addClass("error");
                $("#basic_detail").attr('disabled', true).css('opacity', 0.3);
            }
        }
    };

    // on blur: validate
    telInput.blur(function() {
        reset();
    });

    // on keyup / change flag: reset
    telInput.on("keyup change", reset);

});
</script>

<template>
    <div class="relative">
        <input type="text" autocomplete="phone_number" id="phone" class="block w-full xl:w-full rounded-2 border border-solid border-ash-350 bg-white px-3 pt-2 pb-2.5 text-ash-750 dark:bg-gray-700 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-primary focus:outline-none focus:ring-0 focus:border-primary peer" />
        <InputError class="mt-2" :message="phone_number" />
        <div class="input-errors">
            <InputError class="mt-2" />
        </div>
    </div>
</template>

<style>
.intl-tel-input {
    width: 100% !important;
}
.intl-tel-input .country-list{
    z-index: 20 !important;
}
.intl-tel-input.separate-dial-code.allow-dropdown.iti-sdc-3 input, .intl-tel-input.separate-dial-code.allow-dropdown.iti-sdc-3 input[type="text"], .intl-tel-input.separate-dial-code.allow-dropdown.iti-sdc-3 input[type="tel"] {
    padding-left: 92px !important;
    font-size: 14px;
}

.intl-tel-input.separate-dial-code.allow-dropdown.iti-sdc-4 input, .intl-tel-input.separate-dial-code.allow-dropdown.iti-sdc-4 input[type="text"], .intl-tel-input.separate-dial-code.allow-dropdown.iti-sdc-4 input[type="tel"]{
    font-size: 14px !important;
}
.intl-tel-input.separate-dial-code .selected-dial-code {
  display: table-cell;
  vertical-align: middle;
  padding-left: 25px;
  font-size: 14px;
}
@media (max-width: 991px) {
    .intl-tel-input.separate-dial-code .selected-dial-code {
        display: table-cell;
        vertical-align: middle;
        padding-left: 12px !important; ;
    }
}

@media (max-width: 500px) {
    .intl-tel-input .country-list{
        width: 300px !important;
    }
}
</style>
