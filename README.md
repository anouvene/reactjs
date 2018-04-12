Initiation à la librairie react.js de Facebook
----------------------------------------------


> Pré-requis:

    - Chrome
    - IDE Visual Studio (Pour installer les plugins)  ou autres
    - Serveur Apache Wamp, Mamp ...
    - Le terminal

> Lien vers Facebook react et jsx(javascript and xml):

    https://facebook.github.io/react/
    https://facebook.github.io/jsx

Et comment utiliser jsx aus ein de react : https://facebook.github.io/react/docs/jsx-in-depth.html

> Test react en ligne avec le transpiler Babel: https://babeljs.io/repl

&nbsp;
> Notion composant dans react :

Un composant est un emplacement dans la page et il peut avoir :

    - de la logique métier
    - des sous-composants
    - du templating
    - à certains endroit du DOM un autre composant

&nbsp;
> Créons un premier composant :


    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="UTF-8" />
        <title>Hello React!</title>
        <script src="https://unpkg.com/react@15.3.2/dist/react.js"></script>
        <script src="https://unpkg.com/react-dom@15.3.2/dist/react-dom.js"></script>
        <script src="https://unpkg.com/babel-core@5.8.38/browser.min.js"></script>
      </head>
      <body>
        <div id="mountNode"></div>

        <script type="text/babel">
          // Element ciblé du DOM
          var mountNode = document.getElementById('mountNode');

          // Corps du composant
          class MyComponent extends React.Component {
            render() {
              return (
                <h1>Hello, world!</h1>
              );
            }
          }

          // Générer le rendu du composant dans l'élément cible du DOM
          ReactDOM.render(<MyComponent />, mountNode);
        </script>
      </body>
    </html>

En terme de résultat, c'est la combinaison de jsx et babel qui permettent au composant de fonctionner.