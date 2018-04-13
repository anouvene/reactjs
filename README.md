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

Et comment utiliser jsx au sein de react : https://facebook.github.io/react/docs/jsx-in-depth.html

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
            construct() {
               super();
               this.state = { name : "Antoine" };
            }

            render() {
              return (
                <h1>Hello { this.state.name } !</h1>
              );
            }
          }

          // Générer le rendu du composant dans l'élément cible du DOM
          ReactDOM.render(<MyComponent name="Antoine" />, mountNode);
        </script>
      </body>
    </html>

En terme de résultat, c'est la combinaison de jsx et babel qui permettent au composant de fonctionner.

> Styler un composant

    class MyComponent extends React.Component {
        constructor(){
            super();
            this.state = { name: "Tuan" };
        };

        render() {
            return (
                <h1 style={{color : this.props.color ? this.props.color : 'blue'}}>Hello, {this.state.name}!</h1>
            );
        }
    }

    ReactDOM.render(<MyComponent color="red" name="Antoine" />, mountNode);


> Evènement avec react

    constructor(){
      super();
      this.state = { name: "Tuan" };

      // Penser à attacher le contexte this à la méthode changeName
      this.changeName = this.changeName.bind(this);
    };

    changeName(){
      this.setState({name: 'Minh Tuan'});
    }

    render() {
        return (
          <h1
              style = {{ color : this.props.color ? this.props.color : 'blue' }}
              onClick = { this.changeName }
          >Hello, {this.state.name}!</h1>
        );
    }

> Créer une référence

    1 -  Stocker une instance d'un composant dans une variable pour ensuite la réutiliser ailleurs
    var component = ReactDOM.render(<MyComponent color="red" name="Antoine" />, mountNode);

    2 - Utiliser les références au sein même de la template du composant pour modifier son comportement
    render() {
        return (
          <h1
              style = {{ color : this.props.color ? this.props.color : 'blue' }}
              onClick = { this.changeName }
              ref = {{ (title) => console.log(title) }}
          >Hello, {this.state.name}!</h1>
        );
    }

    3 - Stocker la référence à un élément d'un template dans une propriété du composant pour l'utiliser ailleurs dans une fonction par exemple
    changeName(){
          this.setState({name: 'Minh Tuan'});
          console.log(this.title);
        }
    ...
    ref = { (title) =>  this.title = title }


> Exemple complet: référence + event + data binding bidirectionnel

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
          var mountNode = document.getElementById('mountNode');

          class MyComponent extends React.Component {
              constructor(){
                  super();
                  this.state = { name: 'Tuan', query: '' };

                  // Penser à bind pour que le click pour avoir lieu
                  this.changeName = this.changeName.bind(this);
                  this.update = this.update.bind(this);
              };

              changeName(){
                  this.setState({ name: 'Minh Tuan' });
                  console.log(this.title);
              }

              update() {
                  this.setState( { query: this.input.value } )
              }

              render() {
                  return (
                      <div>
                        <h1
                            style = {{ color : this.props.color ? this.props.color : 'blue' }}
                            onClick = { this.changeName }
                            ref = { (title) =>  this.title = title }
                        >Hello, { this.state.name }!</h1>

                        <input ref = { (input) => this.input = input } onChange = { this.update } />
                        <p>{ this.state.query }</p>
                      </div>
                  );
              }
          }

          ReactDOM.render(<MyComponent color="red" name="Antoine" />, mountNode);
        </script>
      </body>
    </html>






