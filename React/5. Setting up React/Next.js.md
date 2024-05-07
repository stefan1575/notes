# **Next.js**

Next.js is a React framework that handles the tooling and configuration needed to build a React app.

It also provides additional structure, features, and optimizations for your application.

```
npm install react@latest react-dom@latest next@latest
```

When the `npm run dev` command is executed, `layout.js` is automatically created. It is the main layout of your application which is used to create UI elements that are shared across pages such as navbar, footer, etc.

```json
// package.json
{
	"scripts": {
		"dev": "next dev"
	},
	"dependencies": {
		"next": "^14.1.0",
		"react": "^18.2.0",
		"react-dom": "^18.2.0"
	}
}
```

## **Creating your first Page**

Next.js uses file-system routing where instead of using code to define the routes of your application.

Each folder in a route represents a _route segment_ with the `app` folder as the root segment. Each route segment is mapped to the corresponding segment in a URL path.

The initial slash after the domain name represents the root segment, where the child files are prepended after the slash. Nested folders are represented by the folder name and another slash with its children appended and so on.

## **Server vs. Client Components**

The _client_ refers to the browser on the user's device that sends a request to a server for your application code. It recieves a response from a server with an interactable UI.

The _server_ refers to the computer in a data center that stores the application code.
