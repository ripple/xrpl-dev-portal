{{ start_step("Generate") }}
<button id="generate-creds-button" class="btn btn-primary">資格情報を作成する</button>
<div id='loader-0' style="display: none;"><img class='throbber' src="assets/img/xrp-loader-96.png"> Generating Keys...</div>
<div id='address'></div>
<div id='secret'></div>
<div id='balance'></div>
<div id="populate-creds-status"></div>
{{ end_step() }}
<script type="application/javascript">
$(document).ready( () => {

  $("#generate-creds-button").click( () => {
    // Wipe existing results
    $("#address").html("")
    $("#secret").html("")
    $("#balance").html("")
    $("#populate-creds-status").html("")

    $("#loader-0").show()

    $.ajax({
      url: "https://faucet.altnet.rippletest.net/accounts",
      type: 'POST',
      dataType: 'json',
      success: function(data) {
        $("#loader-0").hide()
        $("#address").hide().html("<strong>Address:</strong> " +
          '<span id="test-net-faucet-address">' +
          data.account.address
          + "</span>").show()
        $("#secret").hide().html('<strong>Secret:</strong> ' +
          '<span id="test-net-faucet-secret">' +
          data.account.secret +
          "</span>").show()
        $("#balance").hide().html('<strong>Balance:</strong> ' +
          Number(data.balance).toLocaleString('en') +
          ' XRP').show()

        // Automatically populate examples with these credentials...
        // Set sender address
        let generated_addr = ""
        $("code span:contains('"+EXAMPLE_ADDR+"')").each( function() {
          let eltext = $(this).text()
          $(this).text( eltext.replace(EXAMPLE_ADDR, data.account.address) )
        })

        // Set sender secret
        $("code span:contains('"+EXAMPLE_SECRET+"')").each( function() {
          let eltext = $(this).text()
          $(this).text( eltext.replace(EXAMPLE_SECRET, data.account.secret) )
        })

        $("#populate-creds-status").text("Populated this page's examples with these credentials.")

        complete_step("Generate")

      },
      error: function() {
        $("#loader-0").hide();
        alert("There was an error with the Ripple Test Net, please try again.");
      }
    })
  })

  const EXAMPLE_ADDR = "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe"
  const EXAMPLE_SECRET = "s████████████████████████████"
  $("#populate-creds-button").click( () => {

  })

})
</script>

**注意:** Rippleは[XRP Ledger Testnet](parallel-networks.html)をテストの目的でのみ運用しており、Test Netの状態とすべての残高を定期的にリセットしています。予防措置として、Test Netと本番で同じアドレスを使用**しない**ことをお勧めします。