# hindrance

A convenience library for working with a particularly inconvenient authentication mechanism (OAuth 2.0 JWT Bearer Credentials Flow).

## Usage

This library provides a wrapper function for [clj-http](https://github.com/dakrone/clj-http) client functions, so that you can simply make requests as you normally would and the bearer access token will be transparently added to the request headers.

In your project.clj: 

```
[kueso/hindrance "0.2.0"]
```

Or if your desires are *unconventional*:

```
<dependency>
  <groupId>thirtyspokes</groupId>
  <artifactId>hindrance</artifactId>
  <version>0.1.3</version>
</dependency>
```

Then:

```clojure
(ns your-project.core
  (:require [hindrance.core :refer [defcreds with-oauth-token]]
            [clj-http.client :as client]))

;; Declare your client-id, shared-secret, and your oauth provider's url
(defcreds "your-client-id" "your-shared-secret" "your-oauth-url")

;; Assuming you received the access token "super-good-token", the with-oauth-token wrapper
;; will adjust your request map to include Authorization: Bearer super-good-token in the headers.
(with-oauth-token client/get "https://www.some-authenticated-service.com")
```

If you just want the token for some other purpose and don't care about the convenience function:

```clojure
(ns your-project.core
  (:require [hindrance.core :refer [get-access-token]]))

(get-access-token)
;; Will return your access token, as a string.
```

Hindrance will save the token you receive as an atom, and whenever `get-access-token` is called, it will re-use the token if it has not yet expired (based on the expiry time defined by your OAuth token provider), or request a brand-new one if it has.

## License

Copyright © 2016 Ray Ashman Jr.

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
