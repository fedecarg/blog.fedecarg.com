---
title: "How to create a Data Container Component in React"
date: "2018-03-24"
tags: [design-patterns,frameworks,javascript,node-js,react]
---

One pattern I've used quite a lot while working with React at the [BBC](http://bbc.com/news) and [Discovery Channel](https://corporate.discovery.com/) is the **Data Container** pattern. It became popular in the last couple of years thanks to libraries like Redux and Komposer.

![wireframe](https://kewnode.files.wordpress.com/2018/03/wireframe1.jpg)

The idea is simple. When you build UI components in React you feed data into them via containers. Inside those containers you may need to access different data sources, filter data, handle errors, etc. So data containers help you build data-driven components and separate them into two categories: Data components and Presentational components.

- A **Presentational** component is mainly concerned with the view, it doesn't specify how the data is loaded or mutated. They receive data and callbacks exclusively via props.
- A **Data** component talks to the data sources and provides the data and behaviour to the Presentational component. It's usually generated using higher order function, such as connect() or createContainer().

There are actually 2 ways to implement this pattern, using inheritance or composition:

1. **Inheritance**: a React component class extends a Data Container component class.
2. **Composition**: a React component is injected into the Data Container ([React Komposer](https://github.com/arunoda/react-komposer) uses this approach).

I recommend composition over inheritance as a design principle because it gives you more flexibility.

## Code Example

Lets say you want to display a list of notifications and you have 2 components: **NotificationsContainer** and **NotificationsList**

First you need to fetch the data and add it to the NotificationsContainer:

```js
import React, {createElement} from 'react';
import PropTypes from 'prop-types';
import https from 'https';
import DataStore from '/path/to/DataStore';

export default function createContainer(SubComponent, subComponentProps) {

    class DataContainer extends React.Component {

        constructor(props) {
            super(props);

            this.name = props.name;
            this.dataSourceUrl = props.dataSourceUrl;

            this.state = {
                data: null,
                error: null
            };
        }

        componentDidMount() {
            this.setInitialData();
        }

        setInitialData() {
            if (DataStore.hasData(this.name)) {
                this.setState({
                    data: DataStore.getData(this.name)
                });
            } else {
                this.fetchData();
            }
        }

        fetchData() {
            https.get(this.dataSourceUrl, res => {
                let chunkedData = '';

                res.on('data', data => {
                    chunkedData += data;
                });

                res.on('end', () => {
                    this.setState({
                        data: chunkedData
                    });
                });

                res.on('error', (error) => {
                    this.setState({error});
                });
            });
        }

        render() {
            return createElement(
                SubComponent,
                Object.assign({}, subComponentProps, this.state)
            );
        }
    }

    DataContainer.propTypes = {
        name: PropTypes.string,
        dataSourceUrl: PropTypes.string
    };

    return DataContainer;
}
```

Then you need to create a NotificationsList component that receives the data as a prop:

```js
import React from 'react';
import PropTypes from 'prop-types';

class NotificationsList extends React.Component {

    constructor(props) {
        super(props);
    }

    render() {
        const listItems = this.props.data.items || [];

        return (
            <ul>
                {listItems.map((item, index) => {
                    return <NotificationListItem item={item} index={index} />;
                })}
            </ul>
        );
    }
}

NotificationsList.propTypes = {
    data: PropTypes.object,
    error: PropTypes.object
};

export default NotificationsList;
```

And, finally, you need to create and render the data container:

```js
import React from 'react';
import NotificationsList from './NotificationsList';
import createContainer from './createContainer';

export default class HomePage extends React.Component {

    render() {
        const NotificationsContainer = createContainer(
            NotificationsList, {
                propName: 'propValue'
            }
        );

        return (
            <NotificationsContainer
                dataSourceUrl="/api/notifications/list"
                name="notifications" />
        );
    }
}
```

If you are looking for something a bit more advanced, similar to what I was using at the BBC, then check out this nice little project called  [Second](https://github.com/wildlyinaccurate/second/). Or, if you are building a more complex app and need to manage state or map components to multiple containers, then you should consider using Redux. Here's a great [presentation about React/Redux](http://blog.isquaredsoftware.com/presentations/2018-03-react-redux-intro/#/).

For those using React 16.3, keep an eye on the following projects: [react-waterfall](https://github.com/didierfranc/react-waterfall) and [unistore](https://github.com/developit/unistore). They are data stores built on top of the new [Context API](https://reactjs.org/docs/context.html).

If you don’t want to miss any of my articles, follow me on twitter [@fedecarg](https://twitter.com/fedecarg)
