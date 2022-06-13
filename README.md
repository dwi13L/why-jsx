Exploring how coding in react would be without the tooling and JSX.

### Here's the JavaScript code behind the scenes

```js
/*
 * Why JSX?
 */

// A stateless functional component
const Person = ({ name, phone, email }) => {
  return React.createElement("div", { class: "contact" }, [
    React.createElement("h2", {}, name),
    React.createElement("div", { class: "details" }, [
      React.createElement("p", {}, "Phone: " + phone),
      React.createElement("p", {}, "Email: " + email),
    ]),
  ]);
};

// A Stateful functional component
const App = () => {
  const [contacts, setContacts] = React.useState([]);

  React.useEffect(() => {
    (async () => {
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/users`
      );

      if (response.status !== 200) {
        //invoke handler
        return;
      }

      const users = await response.json();
      setContacts(users);
    })();
  }, []);

  /*
   * createElement takes in 3 args
        1. component - A String or a Component 
        2. props object for the component passed in as arg1- {}
        3. children
   */

  return React.createElement("div", { class: "app" }, [
    React.createElement("div", { class: "title-container" }, [
      React.createElement("h1", { class: "title" }, "Contacts"),
    ]),
    React.createElement("div", { class: "contacts-container" }, [
      contacts.map((user) =>
        React.createElement(
          Person,
          { key: user.id, ...user },
          null /* Optional argument */
        )
      ),
    ]),
  ]);

  /**
   
   So to answer the question, why JSX?
   
    As we can see, encapulating the underlying javascript code with JSX
     + improves the developer experience
     + code readablility
    by a large extent
   
   */
};

// Rendering to DOM with React-DOM
const container = document.getElementById("root");
const root = ReactDOM.createRoot(container);
root.render(React.createElement(App));

```