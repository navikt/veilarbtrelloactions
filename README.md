# veilarbtrelloactions
*Github actions for å hente Trello kort fra kortnummer, og laste opp branch/commit til kortet.* </br>
Bruker Trello sitt API for å manipulere kort: https://developer.atlassian.com/cloud/trello/guides/rest-api/api-introduction/ 

Bruk denne lenken for å generere en key og et token for din trello bruker: https://trello.com/app-key </br>
Finn board id fra url i trello brettet.

Alle meldinger som starter med TC-<num> vil lenkes til et trellokort på brette. F.eks. TC-123 til trello kort 123.

Eksempel av en "github action job" for å kunne lenke branch fra commit melding til Trello kort:

```
link-to-trello:
        runs-on: ubuntu-latest
        name: Trello update
        steps:
            - name: Checkout
              uses: actions/checkout@v2
            - name: Get trello card id
              id: card
              uses: navikt/veilarbtrelloactions/get-card@v1.0
              with:
                  key: ${{ secrets.TRELLO_KEY }}
                  token: ${{ secrets.TRELLO_TOKEN }}
                  board: ${{ secrets.TRELLO_BOARD_ID }}
            - name: Attach branch to card
              id: Attach
              uses: navikt/veilarbtrelloactions/create-attachment@v1.0
              with:
                  key: ${{ secrets.TRELLO_KEY }}
                  token: ${{ secrets.TRELLO_TOKEN }}
                  card-id: ${{ steps.card.outputs.card-id }}
                  attachment-type: branch
```
attachment-type kan også ta verdien commit.

**Tips:** bruk Autolink references i settings til repository for å lenke direkte til kort: </br>
TC-123  ->  https://trello.com/b/xwFqjmQY/team-voff?menu=filter&filter=123

