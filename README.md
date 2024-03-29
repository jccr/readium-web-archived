# Readium Web

Guidelines for an implementation of the Readium Architecture for Web-based platforms.

**Editors:**

- Juan Corona ([Evident Point](https://evidentpoint.com))

**Participate:**

- [GitHub jccr/readium-web](https://github.com/jccr/readium-web)
- [File an issue](https://github.com/jccr/readium-web/issues)

**Document status:** `DRAFT`

## 1. Introduction

### 1.1 Goals

- Build on the [Open Web Platform](https://www.w3.org/wiki/Open_Web_Platform)
- Follow modern web development practices
- Pragmatic use of latest web standards
- Use with or _without_ a Readium Publication Server
- Support offline use cases
- Consider limited connectivity, low-spec clients
- Compatible with [Assistive Technologies](https://www.w3.org/WAI/people-use-web/tools-techniques/)
- Mindful of performance [jank](http://jankfree.org/), focus on speed

### 1.2 Non-Goals

- User Interface implementations
- Browser incompatible specific APIs (Node.js)

## 2. Implementations

### 2.1 Models

- Publication
- Locators

### 2.2 Main Modules

- HTML Navigator
- Audio Navigator
- Image Navigator

## 3. Processing Publications

Unpacked publications can be served from an HTTP web server, with resources as static assets.

Packaged publications can be streamed from a Publication Server.

To process a manifest you can start with JavaScript's native JSON parsing to deserialize into a simple object.

With the module that implements a Publication Model you can read and manipulate a model that's validated against the [Readium Web Publication schema](https://readium.org/webpub-manifest/#appendix-a-json-schema), with optional typing (via TypeScript) and with the addition of convenience methods.

## 4. Fetching Resources

Resources are fetched from a server given a [web origin](https://readium.org/architecture/server/origin.html). Typically via HTTPS, initiated by the HTML processing model, and programmatically using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

### 4.1 Download Constraints

Publication resources are streamed or downloaded over networking protocols.

Contrary to other implementations that run on devices with file-system access, publication resources on the web are not, as a whole, directly or immediately accessible.

## 5. Loading Content

Publication resources generally need to be loaded (embedded) into the render tree (DOM) of a web-based reading system.

### 5.1 Embedding
List of content types with suggested embed elements, or other:

- HTML documents: [`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe), or other means of document [transclusion](https://en.wikipedia.org/wiki/Transclusion)
- Audio and video: [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio), [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video)
- Images and graphics: [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img), [`<picture>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture), [`<svg>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/svg), [`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas), [`background-image` CSS property](https://developer.mozilla.org/en-US/docs/Web/CSS/background-image)

## 6. Presenting Content

Sequencing and layout of content in reading order progression.

Match [Presentation Hints](https://readium.org/webpub-manifest/extensions/presentation.html).

Fragment, paginate and apply synthetic spreads.

### 6.1 Prefetching and Virtualization

Virtualized pagination and scrolling.

Recommended approach for implementing [continuous](https://readium.org/webpub-manifest/extensions/presentation.html#continuous) [overflow](https://readium.org/webpub-manifest/extensions/presentation.html#overflow).

### 6.2 Performance Constraints

When loading or rendering additional resources for the purposes of prefetching and virtualization, consider frame times, DOM node count, CPU and memory heap.

Given the linear progression of a user's reading activity it is advisable to limit the prefetching to the adjacent (previous and next) resources of a resource  set in the reading progression. To effectively benefit from this approach, the adjacent resources can be fetched with high priority, as soon as or after the primary resource to render is ready. 

The prefetch range could increase, with lower priority prefetching, given the known or inferred capabilities of the connected client.
