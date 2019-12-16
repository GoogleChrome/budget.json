# Budget.json


## This repo

This repository tracks the current `budget.json` format as well as upcoming changes and features. `budget.json` is the format used by [Lighthouse](https://github.com/GoogleChrome/lighthouse) for declaring performance budgets.

NOTE: The Lighthouse npm module is updated periodically, so it doesn't necessarily reflect very recent code changes. Features affected by this are noted in the API docs below. If you don't want to wait for the updated npm package, you can install Lighthouse directly from [Github](https://github.com/GoogleChrome/lighthouse) to get the most up-to-date code.

## Sample budget.json file
The `budget.json` file is an array containing one or more `Budget` objects.

This sample `budget.json` file below contains two `Budget` objects:
- The **first budget** applies to all pages *except* those with URL path starting with `/blog`. The budget sets the following thresholds: a 300KB limit on total page resources, a 150KB limit on total JavaScript resources, and a limit of 10 third-party requests.
- The **second budget** that applies to pages with a URL path starting with `/blog`. The budget sets the following thresholds: a 200KB limit on total page resources.

```json
[
   {
      "path":"/*",
      "resourceSizes":[
         {
            "resourceType":"total",
            "budget":300
         },
         {
            "resourceType":"script",
            "budget":150
         }
      ],
      "resourceCounts":[
         {
            "resourceType":"third-party",
            "budget":10
         }
      ]
   },
   {
      "path":"/blog",
      "resourceSizes":[
         {
            "resourceType":"total",
            "budget":200
         }
      ]
   }
]
```


## The Budget Object

This section provides more information on the properties that are a part of the `Budget` object.

### path

_Optional, String, Lighthouse 5.3 & up_

The `path` property indciates the pages that a budget applies to. This string should follow the [robots.txt](https://developers.google.com/search/reference/robots_txt#examples-of-valid-robotstxt-urls) format.

If `path` is not supplied, a budget will apply to all pages.

If a page's URL path matches the `path` property of more than one budget in `budget.json`, then the last matching budget will be applied. As a result, global budgets (e.g. `"path": "/*"`) should be listed first in `budget.json`, followed by the budgets that override the global budget (e.g. `"path": "/blog"`). 

**Examples**

Match all URL paths.

`"path": "/"` (This is equivalent to writing `"path": "/*"`)

Match all URL paths starting with `/articles`.

`"path": "/articles"`

Match URL paths within the `uk/` directory and ending with `shopping-cart`.

`"path": "/uk/*/shopping-cart$"`


### resourceSizes

_Optional, Array_

This is an array of `resourceSizeBudget` objects. A `resourceSizeBudget` object contains two properties: `resourceType` and `budget`. `resourceType` is one of the [resource types](#resourcetype) supported by `budget.json`. The `budget` property is defined in kilobytes and refers to the _transfer size_ of a resource.

**Example**

This array contains two resource size budgets: a budget of 500KB for total resources &  a budget of 300KB for script resources.

```json
[
   {
      "resourceType":"total",
      "budget":500
   },
   {
      "resourceType":"script",
      "budget":300
   }
]
```


### resourceCounts

_Optional, Array_

This is an array of `resourceCountBudget` objects. A `resourceCountBudget` object consists of two required properties: `resourceType` and `budget`. `resourceType` is one of the [resource types](#resourcetype) supported by `budget.json`. `budget` is defined in number of requests..

**Example**

This array contains one resource count budget: a budget of 10 requests for third-party resources.

```json
[
   {
      "resourceType":"third-party",
      "budget":10
   }
]

```


### resourceType

_String_

The following resource types are supposed by `budget.json`:



*   `document`
*   `font`
*   `image`
*   `media`
*   `other`
*   `script`
*   `stylesheet`
*   `third-party`
*   `total`


## Proposed Features



*   Support setting budgets for specific files.
*   Support whitelisting of first-party domains.
*   Support timing-based budgets.
*   Support setting budgets for the _change_ in resource size(s).


## Contributing

If you have feedback or ideas on the `budget.json` format, please file an issue in this repo to get involved.

If you are interested in contributing to the Lighthouse `budget.json` implementation, please refer to the [Lighthouse](https://github.com/GoogleChrome/lighthouse) repo.

