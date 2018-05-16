**En chiffres** | Depuis 24H | Depuis 7 jours | Depuis 1 mois | Depuis 1 an
--- | --- | --- | --- | ---
Somme donnée aux bénéficiaires (en ethers) | 0 | 0 | 0 | 0
Dons reçus (nombre) | 0 | 0 | 0 | 0
Inscriptions de bénéficiaires (nombre) | 0 | 0 | 0 | 0
Désinscriptions de bénéficiaires (nombre) | 0 | 0 | 0 | 0

# Dernières transactions du contrat

- Don de 0.00 ETH par le donateur 0x000...
- Don de 0.00 ETH par le donateur 0x000...
- Distribution de 0.00 ETH à 0 bénéficiaires
- Inscription du bénéficiaire 0x000...
- Encaissement de 0.00 ETH
- Inscription du bénéficiaire 0x000...
- Inscription du bénéficiaire 0x000...
- Désinscription du bénéficiaire 0x000...
- Don de 0.00 ETH par le donateur 0x000...
- Don de 0.00 ETH par le donateur 0x000...
- Distribution de 0.00 ETH à 0 bénéficiaires
- Don de 0.00 ETH par le donateur 0x000...
- Don de 0.00 ETH par le donateur 0x000...
- Inscription du bénéficiaire 0x000...
- Don de 0.00 ETH par le donateur 0x000...
- Don de 0.00 ETH par le donateur 0x000...

<div id="transactions" />
[Audit technique du contrat et des transactions](https://etherscan.io/address/0xd972634e4a036d91d0d4a35ef4927b63ac0fa7f4)

## Pour donner aux bénéficiaires inscrits sur le contrat, envoyez vos ethers à l'adresse suivante

### 0x000...

<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script>
    var etherscanAPIKeyToken = "MyApiKeyToken";
    var contract_address = "0xd972634e4a036d91d0d4a35ef4927b63ac0fa7f4";
    var balance_request = "module=account&action=balance&address="
        + contract_address
        + "&tag=latest";
    var relative_url_of_incoming_transactions_request = "module=account&action=txlist&address="
        + contract_address
        + "&startblock=0&endblock=99999999&page=1&offset=10&sort=asc"
    var relative_url_of_outgoing_transactions_request = "module=account&action=txlistinternal&address="
        + contract_address
        + "&startblock=0&endblock=99999999&page=1&offset=10&sort=asc"
    var absolute_url_of_incoming_transactions_request = "https://api.etherscan.io/api?"
        + relative_url_of_incoming_transactions_request
        + "&apikey="
        + etherscanAPIKeyToken;
    var absolute_url_of_outgoing_transactions_request = "https://api.etherscan.io/api?"
        + relative_url_of_outgoing_transactions_request
        + "&apikey="
        + etherscanAPIKeyToken;
    var transactions = [];
    $.getJSON( absolute_url_of_incoming_transactions_request )
        .done( function(data) {
            console.log( "done", data );
            transactions = data.result;
            // we got incoming transactions, let's get outgoing transactions too
            $.getJSON( absolute_url_of_outgoing_transactions_request )
                .done( function(data) {
                    console.log("done2", data);
                    transactions = transactions.concat(data.result);
                    // we got all transactions from and to the contract
                    // sort them by timestamp
                    transactions.sort( function(t1, t2) { return t2.timestamp - t1.timestamp; } );
                    var html = '<ul>';
                    transactions.forEach(function(item, index, array) {
                        console.log(item, index);
                        var newDate = new Date();
                        newDate.setTime(item.timeStamp*1000);
                        dateString = newDate.toISOString();
                        html += '<li><a href="https://etherscan.io/tx/' + item.hash + '">' +
                            dateString.substring(0,10) + ' ' +
                            dateString.substring(11,19) + ' : transaction ' +
                            item.hash.substring(0, 6) + '...</a></li>';
                    });
                    html += '</ul>';
                    html += '<p><a href="https://etherscan.io/address/' + contract_address ;
                    html += '">Audit technique du contrat et des transactions</a></p>';
                    $('#transactions').html(html);
                } )
                .fail( function(error) { console.log("fail2", error); } )
                .always( function() { console.log("always2"); } );
            } )
        .fail( function(error) { console.log( "fail", error ); } )
        .always( function() { console.log( "always" ); } );
</script>
