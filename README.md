Initiation à la librairie react.js de Facebook
----------------------------------------------


> Pré-requis:

    - Chrome
    - IDE Visual Studio (Pour installer les plugins)  ou autres
    - Plugins utiles : Reactjs code snippets, JSX support(déjà installé par défaut dans VS code)
    - Serveur Apache Wamp, Mamp ...
    - Le terminal

> Lien vers Facebook react et jsx(javascript and xml):

    https://facebook.github.io/react/
    https://facebook.github.io/jsx
    => JSX est une extension syntaxique pour Javascript et ReactJS est basé sur JSX 
    à la manière d'Angular avec TypeScript.
    
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


&nbsp;
> Créer une référence

    1 -  Stocker une instance d'un composant dans une variable pour ensuite la réutiliser ailleurs
    var component = ReactDOM.render(<MyComponent color="red" name="Antoine" />, mountNode);

    2 - Utiliser les références au sein même de la template du composant pour modifier son comportement
    render() {
        return (
          <h1
              style = {{ color : this.props.color ? this.props.color : 'blue' }}
              onClick = { this.changeName }
              ref = {{ (title) => console.log(title) }} // title fait référence à l'élément h1
          >Hello, {this.state.name}!</h1>
        );
    }

    3 - Stocker la référence à un élément d'un template dans une propriété du composant
        pour l'utiliser ailleurs dans une fonction par exemple
    changeName(){
          this.setState({name: 'Minh Tuan'});
          console.log(this.title);
        }
    ...
    ref = { (title) =>  this.title = title }


&nbsp;
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

                  // Penser à bind pour que le click puisse avoir lieu
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


&nbsp;
> Exemple de composant faisant appel à un autre composant
Nous allons modifier l'exemple précédent en créant un composant pour le champ de recherche

    var mountNode = document.getElementById('mountNode');

    // Composant search
    class SearchComponent  extends React.Component {
        constructor(){
            super();
            this.state = { query: ''}
            this.update = this.update.bind(this);
        }

        update() {
            this.setState( { query: this.input.value } )
        }

        render() {
            return (
                <div>
                    <input ref = { (input) => this.input = input } onChange = { this.update } />
                    <p>{ this.state.query }</p>
                </div>
            );
        }
    }

    // Composant main
    class MainComponent extends React.Component {
        constructor(){
            super();
            this.state = { name: 'Tuan'};

            // Penser à bind pour que le click pour avoir lieu
            this.changeName = this.changeName.bind(this);
        };

        changeName(){
            this.setState({ name: 'Minh Tuan' });
            console.log(this.title);
        }

        render() {
            return (
                <div>
                    <h1
                        style = {{ color : this.props.color ? this.props.color : 'blue' }}
                        onClick = { this.changeName }
                        ref = { (title) =>  this.title = title }
                    >Hello, { this.state.name }!</h1>
                    <SearchComponent/>
                </div>
            );
        }
    }

    ReactDOM.render(<MainComponent color="red" name="Antoine" />, mountNode);


> Communication entre composants : utiliser "props" pour échanger les infos entre les composants

    ...

    // Composant search
    class SearchComponent  extends React.Component {
        ...

        update() {
            ...
            // Récupérer la propriété "onChange"
            // Puis on transmet les infos saisis dans input à la méthode "logSearch" du composant parent
            this.props.onChange(this.input.value);
        }

        render() {
            return (
                <div>
                    ...
                    // On affiche le nom du composant parent
                    <p>{ this.props.parentName }</p>
                </div>
            );
        }
    }

    // Composant main
    class MainComponent extends React.Component {
        ...

        // Log infos du input
        logSearch(value){
            console.log(value);
        }

        render() {
            return (
                <div>
                    ...
                    // Injecter la méthode "logSearch" au composant enfant par la propriété "onChange"
                    <SearchComponent parentName={ this.state.name } onChange={ this.logSearch }/>
                </div>
            );
        }
    }

    ReactDOM.render(<MainComponent color="red" name="Antoine" />, mountNode);


&nbsp;
> Cycle de vie d'un composant

    // avant le rendu, peut écraser le state avec un setState
    componentWillMount() {
      console.log('componentWillMount');
    }

    // après le rendu, on accède aux refs
    componentDidMount() {
      console.log('componentDidMount');
    }

    // à la mise à jour des props, on peut aussi faire un setState sans rendu supplémentaire
    componentWillReceiveProps() {
      console.log('componentWillReceiveProps');
    }

    // à la mise à jour props ou state, on peut retourner false pour ne pas faire de rendu
    shouldComponentUpdate() {
      console.log('shouldComponentUpdate');

      return true;
    }

    // à la mise à jour avant le rendu, on peut préparer avant le rendu
    componentWillUpdate() {
      console.log('componentWillUpdate');
    }

    // après le rendu, on accède au nouveau DOM
    componentDidUpdate() {
      console.log('componentDidUpdate');
    }

    // après le démontage, on peut faire du nettoyage ici
    componentWillUnmount() {
      console.log('componentWillUnmount');
    }

&nbsp;
> Ecrire plus proprement le rendu d'un composant : https://facebook.github.io/react/docs/top-level-api.html

    Avant:
    ReactDOM.render(<MainComponent color="red" name="Antoine" />, mountNode);

    Après:
    let MainComponentElement = React.createElement(MainComponent, {color: "blue"});
    ReactDOM.render(MainComponentElement, mountNode);


&nbsp;
> Méthode statique

    class MainComponent extends React.Component {
            constructor() {
              super();
              // Appel méthode statique
              this.state = MainComponent.getData();
              ...
            }

            static getData() {
              return {actual: "Julien", users: ["Julien", "Christophe", "Roby"], items: []};
            }
    }

    // Utilisation de la méthode statique en dehors de la classe
    console.log(MainComponent.getData());


&nbsp;
> Méthode permettant de fixer par défaut les valeurs des paramètres(props) d'un composant :

    Exemple, au lieu de fixer la proppriété "color" par défaut via le ternaire:
    class MainComponent extends React.Component {
        render(<strong style={{ color:this.props.color ? this.props.color : 'blue' }}>...</strong>);
    }

    // On va plutôt utiliser la méthode "defaultProps" du composant :
    class MainComponent extends React.Component {
        render(<strong style={ {color: this.props.color} }>Hello world</strong>);
    }
    MainComponent.defaultProps = {color: 'red'};

    ...
    // Rendu du composant "main"
    let MainComponentElement = React.createElement(MainComponent, {color: 'blue'}); // Ecrase "defaultProps"
    ReactDOM.render(MainComponentElement, mountNode);

&nbsp;
> Méthode de validation des proppriétés avec "PropTypes" : https://facebook.github.io/react/docs/reusable-components.html


    MainComponent.PropTypes = {
        // attribut color du composant doit être un string
        color: React.PropTypes.string;

        // Ou rendre obligatoire le parametre color dans le composant "MainComponent"
        color: React.PropTypes.string;.isRequired;
    }


&nbsp;
> React Router :
    https://github.com/ReactTraining/react-router
    https://github.com/ReactTraining/react-router/blob/v2.8.1/docs/guides/IndexRoutes.md

    // Ajouter ce lien en entete de page pour disposer de la librarie react-router:
    <script src="https://unpkg.com/react-router@2.8.1/umd/ReactRouter.min.js"></script>


    // Ajouter dans le script de l'application, les Router déclarations suivantes:
    var Router = ReactRouter.Router;
    var Route = ReactRouter.Route;
    var DefaultRoute = ReactRouter.DefaultRoute;
    var Link = ReactRouter.Link;
    var RouteHandler = ReactRouter.RouteHandler;

    // Composant Menu + balise spécifique Link
    // l'attribut activeClassName indique qu'on est dans le rendu du composant courant
    class MenuComponent extends React.Component {
        render() {
            return (
                <ul>
                    <li><Link to={`/`} activeClassName="active">Home</Link></li>
                    <li><Link to={`/users`} activeClassName="activeUsers">Users</Link></li>
                    <li><Link to={`/users/user`} activeClassName="activeUsers">User 1</Link></li>
                </ul>
            );
        }
    }

    // Composant Users
    class UsersComponent extends React.Component {
        render() {
            return (
                <div>
                    <h1>Liste des utilisateurs</h1>
                    <ul>
                        <li>User 1</li>
                        <li>User 2</li>
                        <li>User 3</li>
                    </ul>
                </div>
            );
        }
    }

    // Composant User : details utilisateur
    class UsersComponent extends React.Component {
        render() {
            return (
                <div>
                    <h1>User 1</h1>
                    <details>
                        <summary>Détails utilisateur 1</summary>
                        <p>All content and graphics on this web site are
                        the property of the company Refsnes Data.</p>
                    </details>
                </div>
            );
        }
    }

    // Composant principal Main
    class MainComponent extends React.Component {
        render() {
            return (
                ...
            );
        }
    }

    ...

    // Exemple d'utilisation:
    ReactDOM.render((
        <Router>
            <Route path="/" component={MainComponent} />
            <Route path="/users" component={UsersComponent} />
        </Router>
    ), mountNode);

    // Route imbriquee
    <Router>
        <Route path="/" component={MainComponent} />
        <Route path="users">
            <IndexRoute component={UsersComponent} />
            <Route path="user" component={UserComponent} />

            // Avec parametre
            <Route path=":id" component={UserComponent} />
        </Route>
    </Router>

    // Lien vers une route avec parametre : ex.: /users/1
    <Link to={`users/${userID}`}> {user.name} </Link>

    // Pour récupérer le parametre : props.routeParams.id
    class UserComponent extends React.Component
    {
        constructor(props)
        {
            super(props);
            this.user = UsersComponent.getData().users[props.routeParams.id];

        }
        ...
    }

&nbsp;
> Redirection avec "props.history.push"

    Exemple:
    constructor(props)
    {
        super(propos);
        ...
    }

    componentWillMount()
    {
        // redirection vers la route "/users" du composant UserComponent
        this.props.history.push('/users');
    }


&nbsp;
> Autres solutions pour implémenter une route

    // Pour les petites applications
    react-router-component : github.com/STRML/react-router-component
    react-mini-router : github.com/larrymyers/react-mini-router

    // Pour les applications complexes
    universal-router : www.kriasoft.com/universal-router
    router5.github.io/docs/understanding-router5.html



> Pattern flux

                      <------  Action <-----
                    |                       |
    Action --> Dispatcher --> Store * --> View

    * Store unique : équivaut à un state pour stocker les données
    Et on ne peut agir sur le store (state) que via d'une action(évenement ex.: click)

    + Dispatcher(action) => fait appel à  Reducer(store, action) : gestionnaire d'évènements pour mettre à jour
    les données dans le Store

    * Exemple : *

    var mountNode = document.querySelector('#mountNode');

    let reducer = (store = { value: 0 }, action) => {
      switch (action) {
        case 'INCREMENT':
          store.value++;
          console.log('-- INCREMENT --');
          return store;
        case 'DECREMENT':
          store.value--;
          console.log('-- DECREMENT --');
          return store;
        default:
          console.log('-- INITIAL STORE --');
          return store;
      }
    };

    class Counter extends React.Component {
      constructor() {
        super();
        this.state = reducer(undefined, null);
        this.increment = this.increment.bind(this);
        this.decrement = this.decrement.bind(this);
      }

      dispatch(action) {
        this.setState(prevState => reducer(prevState, action));
      }

      increment() {
        this.dispatch('INCREMENT');
      };

      decrement() {
        this.dispatch('DECREMENT');
      };

      render() {
        return (
          <div>
            {this.state.value}
            <button onClick={this.increment}>+</button>
            <button onClick={this.decrement}>-</button>
          </div>
        )
      }
    }

    var CounterElement = React.createElement(Counter);

    ReactDOM.render(CounterElement, mountNode);




