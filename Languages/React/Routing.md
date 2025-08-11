React uses a react-router-dom npm package to pull this off
react-router-dom gives us predefined components and functions to define out routes such as

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { createBrowserRouter, RouterProvider, Link, Outlet, useParams } from 'react-router-dom';

// createBrowserRouter helps us define structure of routes like what to display when certain route gets visited
// RouterProvider is a wrapper that enables router inside its children elements
// Link tag is a component that is used to navigate internal links which helps react optimise renders rather then re rendering entire page

function AppLayout(){
	const params=usePrarms();
	const { resID }=prarms;
	return (
		<Header/>
		<Outlet/> // This is where childrens will be rendered
	)
} 

const appRouter = createBrowserRouter([
	{
		path:'/',
		element:<AppLayout/>,
		childern=[
			{
				path:'/cart',
				element:<Cart/>
			},
			{
				path:'/checkout',
				element:<Checkout/>
			 },
		]
	}
	{
		path:'about',
		element:<About/>
	},
	{
		path:'restaurents/:id',
		element:<Restaurent/>
	},
	{
		path:'*',
		element:<div>Path not found<div/>,
		errorElement:<div>Custom 404 Error Page</div>,
	}
]);

root.render(<RouterProvider router={appRouter} />);
```

#### Server Side routing v/s Client side routing 
- Server-side = Server sends new pages, full reloads, good for SEO.
- Client-side = JavaScript handles navigation on the client, no full reloads, better user experience but requires more planning


