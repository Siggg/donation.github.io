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
    var relative_url_of_transactions_request = "module=account&action=txlist&address="
        + contract_address
        + "&startblock=0&endblock=99999999&page=1&offset=10&sort=asc"
    var absolute_url_of_transactions_request = "https://api.etherscan.io/api?"
        + relative_url_of_transactions_request
        + "&apikey="
        + etherscanAPIKeyToken;
    $.getJSON( absolute_url_of_transactions_request )
        .done( function(data) {
            console.log( "done", data );
            // we got incoming transactions, let's get outgoing transactions too
            // sort them by timestamp
            var transactions = data.result.sort( function(t1, t2) { return t2.timeStamp - t1.timeStamp; } );
            var html = '<ul>';
            transactions.forEach(function(item, index, array) {
                console.log(item, index);
                var newDate = new Date();
                newDate.setTime(item.timeStamp*1000);
                var dateString = newDate.toISOString();
                var method = item.input.substring(0,10);
                switch(method) {
                    case '0x':
                        var value = Number.parseFloat(item.value / Math.pow(10,18)).toPrecision(5);
                        method = "Réception d'un don de " + value + " ETH";
                        break;
                    case '0x6b9f96ea':
                        method = "Distribution des dons";
                        break;
                    case '0xcdd8b2b2':
                        var beneficiary = item.input.substring(34,38);
                        method = "Inscription du bénéficiaire #" + beneficiary;
                        break;
                    case '0x71d0028d':
                        var beneficiary = item.input.substring(34,38);
                        method = "Désinscription du bénéficiaire #" + beneficiary;
                        break;
                    case '0x60606040':
                        method = "Initialisation du contrat";
                        break;
                    default:
                        method = item.input;
                };
                html += '<li><a href="https://etherscan.io/tx/' + item.hash + '">' +
                    dateString.substring(0,10) + ' ' +
                    dateString.substring(11,19) + ' : ' +
                    method + '</a></li>';
                });
                html += '</ul>';
                html += '<p><a href="https://etherscan.io/address/' + contract_address ;
                html += '">Audit technique du contrat et des transactions</a></p>';
                $('#transactions').html(html);
        } )
        .fail( function(error) { console.log( "fail", error ); } )
        .always( function() { console.log( "always" ); } );
</script>
