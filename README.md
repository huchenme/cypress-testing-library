<div align="center">
<h1>Cypress Testing Library</h1>

<a href="https://www.emojione.com/emoji/1f405">
  <img
    height="80"
    width="80"
    alt="tiger"
    src="https://raw.githubusercontent.com/testing-library/cypress-testing-library/master/other/tiger.png"
  />
</a>

<p>Simple and complete custom Cypress commands and utilities that encourage good
testing practices.</p>

[**Read the docs**](https://testing-library.com/cypress) |
[Edit the docs](https://github.com/alexkrolick/testing-library-docs)

</div>

<hr />

[![Build Status][build-badge]][build]
[![Code Coverage][coverage-badge]][coverage]
[![version][version-badge]][package] [![downloads][downloads-badge]][npmtrends]
[![MIT License][license-badge]][license]

[![All Contributors](https://img.shields.io/badge/all_contributors-25-orange.svg?style=flat-square)](#contributors)
[![PRs Welcome][prs-badge]][prs] [![Code of Conduct][coc-badge]][coc]

[![Watch on GitHub][github-watch-badge]][github-watch]
[![Star on GitHub][github-star-badge]][github-star]
[![Tweet][twitter-badge]][twitter]

<div align="center">
  <a href="https://testingjavascript.com">
    <img
      width="500"
      alt="TestingJavaScript.com Learn the smart, efficient way to test any JavaScript application."
      src="https://raw.githubusercontent.com/testing-library/cypress-testing-library/master/other/testingjavascript.jpg"
    />
  </a>
</div>

## The problem

You want to use [`DOM Testing Library`][dom-testing-library] methods in your
[Cypress][cypress] tests.

## This solution

This allows you to use all the useful
[`DOM Testing Library`][dom-testing-library] methods in your tests.

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Installation](#installation)
  - [With TypeScript](#with-typescript)
- [Usage](#usage)
  - [Differences from DOM Testing Library](#differences-from-dom-testing-library)
- [Other Solutions](#other-solutions)
- [Contributors](#contributors)
- [LICENSE](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installation

This module is distributed via [npm][npm] which is bundled with [node][node] and
should be installed as one of your project's `devDependencies`:

```
npm install --save-dev @testing-library/cypress
```

### With TypeScript

Typings are defined in `@types/testing-library__cypress` at [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/testing-library__cypress),
and should be added as follows in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "types": ["cypress", "@types/testing-library__cypress"]
  }
}
```

## Usage

`Cypress Testing Library` extends Cypress' `cy` command.

Add this line to your project's `cypress/support/commands.js`:

```javascript
import '@testing-library/cypress/add-commands'
```

You can now use all of `DOM Testing Library`'s `findBy`, `findAllBy`, `queryBy`
and `queryAllBy` commands.
[See the `DOM Testing Library` docs for reference](https://testing-library.com)

You can find [all Library definitions here](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/testing-library__cypress/index.d.ts).

To configure DOM Testing Library, use the following custom command:

```javascript
cy.configureCypressTestingLibrary(config)
```

To show some simple examples (from
[cypress/integration/query.spec.js](cypress/integration/query.spec.js) or [cypress/integration/find.spec.js](cypress/integration/find.spec.js)):

```javascript
cy.queryAllByText('Button Text').should('exist')
cy.queryAllByText('Non-existing Button Text').should('not.exist')
cy.queryAllByLabelText('Label text', {timeout: 7000}).should('exist')
cy.findAllByText('Jackie Chan').click();

// findAllByText _inside_ a form element
cy.get('form').within(() => {
  cy.findAllByText('Button Text').should('exist')
})
cy.get('form').then(subject => {
  cy.findAllByText('Button Text', {container: subject}).should('exist')
})
cy.get('form').findAllByText('Button Text').should('exist')
```

### Differences from DOM Testing Library

`Cypress Testing Library` supports both jQuery elements and DOM nodes. This is
necessary because Cypress uses jQuery elements, while `DOM Testing Library`
expects DOM nodes. When you pass a jQuery element as `container`, it will get
the first DOM node from the collection and use that as the `container` parameter
for the `DOM Testing Library` functions.

`get*` queries are disabled. `find*` queries do not use the Promise API of
`DOM Testing Library`, but instead forward to the `get*` queries and use Cypress'
built-in retryability using error messages from `get*` APIs to forward as error
messages if a query fails. `query*` also uses `get*` APIs, but disables retryability.

`findAll*` can select more than one element and is closer in functionality to how
Cypress built-in commands work. `findAll*` is preferred to `find*` queries.
`find*` commands will fail if more than one element is found that matches the criteria
which is not how built-in Cypress commands work, but is provided for closer compatibility
to other Testing Libraries.

Cypress handles actions when there is only one element found. For example, the following
will work without having to limit to only 1 returned element. The `cy.click` will
automatically fail if more than 1 element is returned by the `findAllByText`:

```javascript
cy.findAllByText('Some Text').click()
```

If you intend to enforce only 1 element is returned by a selector, the following
examples will both fail if more than one element is found.

```javascript
cy.findAllByText('Some Text').should('have.length', 1)
cy.findByText('Some Text').should('exist')
```

## Other Solutions

I'm not aware of any, if you are please [make a pull request][prs] and add it
here!

## Contributors

Thanks goes to these people ([emoji key][emojis]):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://kentcdodds.com"><img src="https://avatars.githubusercontent.com/u/1500684?v=3" width="100px;" alt=""/><br /><sub><b>Kent C. Dodds</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=kentcdodds" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=kentcdodds" title="Documentation">📖</a> <a href="#infra-kentcdodds" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=kentcdodds" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://sompylasar.github.io"><img src="https://avatars2.githubusercontent.com/u/498274?v=4" width="100px;" alt=""/><br /><sub><b>Ivan Babak</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=sompylasar" title="Code">💻</a> <a href="#ideas-sompylasar" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="http://team.thebrain.pro"><img src="https://avatars1.githubusercontent.com/u/4002543?v=4" width="100px;" alt=""/><br /><sub><b>Łukasz Gandecki</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=lgandecki" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=lgandecki" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/npeterkamps"><img src="https://avatars1.githubusercontent.com/u/25429764?v=4" width="100px;" alt=""/><br /><sub><b>Peter Kamps</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=npeterkamps" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=npeterkamps" title="Documentation">📖</a> <a href="#ideas-npeterkamps" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=npeterkamps" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/airato"><img src="https://avatars3.githubusercontent.com/u/4506749?v=4" width="100px;" alt=""/><br /><sub><b>Airat Aminev</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=airato" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=airato" title="Tests">⚠️</a> <a href="#tool-airato" title="Tools">🔧</a></td>
    <td align="center"><a href="https://www.webiny.com"><img src="https://avatars0.githubusercontent.com/u/5121148?v=4" width="100px;" alt=""/><br /><sub><b>Adrian Smijulj</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=adrian1358" title="Code">💻</a></td>
    <td align="center"><a href="https://www.ossfinder.com"><img src="https://avatars0.githubusercontent.com/u/12230408?v=4" width="100px;" alt=""/><br /><sub><b>Soo Jae Hwang</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/issues?q=author%3Amisoguy" title="Bug reports">🐛</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=misoguy" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=misoguy" title="Tests">⚠️</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/wKovacs64"><img src="https://avatars1.githubusercontent.com/u/1288694?v=4" width="100px;" alt=""/><br /><sub><b>Justin Hall</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=wKovacs64" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=wKovacs64" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/euZebe"><img src="https://avatars3.githubusercontent.com/u/9463809?v=4" width="100px;" alt=""/><br /><sub><b>euzebe</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=euZebe" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/jkdowdle"><img src="https://avatars0.githubusercontent.com/u/19804196?v=4" width="100px;" alt=""/><br /><sub><b>jkdowdle</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=jkdowdle" title="Code">💻</a></td>
    <td align="center"><a href="https://brian.ng"><img src="https://avatars3.githubusercontent.com/u/56288?v=4" width="100px;" alt=""/><br /><sub><b>Brian Ng</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=existentialism" title="Code">💻</a></td>
    <td align="center"><a href="https://karilaari.fi"><img src="https://avatars2.githubusercontent.com/u/2477131?v=4" width="100px;" alt=""/><br /><sub><b>Kari Laari</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=klaari" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/ppi-buck"><img src="https://avatars2.githubusercontent.com/u/37330764?v=4" width="100px;" alt=""/><br /><sub><b>Basti Buck</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=ppi-buck" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/ShimiTheFirst"><img src="https://avatars2.githubusercontent.com/u/25421369?v=4" width="100px;" alt=""/><br /><sub><b>ShimiTheFirst</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/issues?q=author%3AShimiTheFirst" title="Bug reports">🐛</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/omerose"><img src="https://avatars2.githubusercontent.com/u/9358542?v=4" width="100px;" alt=""/><br /><sub><b>omerose</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=omerose" title="Documentation">📖</a></td>
    <td align="center"><a href="http://www.aaronmcadam.com"><img src="https://avatars3.githubusercontent.com/u/37928?v=4" width="100px;" alt=""/><br /><sub><b>Aaron Mc Adam</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=aaronmcadam" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=aaronmcadam" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://twitter.com/karlhorky"><img src="https://avatars2.githubusercontent.com/u/1935696?v=4" width="100px;" alt=""/><br /><sub><b>Karl Horky</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=karlhorky" title="Documentation">📖</a></td>
    <td align="center"><a href="https://twitter.com/NoriSte"><img src="https://avatars0.githubusercontent.com/u/173663?v=4" width="100px;" alt=""/><br /><sub><b>Stefano Magni</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=NoriSte" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=NoriSte" title="Tests">⚠️</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=NoriSte" title="Documentation">📖</a></td>
    <td align="center"><a href="https://github.com/weyert"><img src="https://avatars3.githubusercontent.com/u/7049?v=4" width="100px;" alt=""/><br /><sub><b>Weyert de Boer</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=weyert" title="Code">💻</a></td>
    <td align="center"><a href="https://simjes.dev/"><img src="https://avatars0.githubusercontent.com/u/6494049?v=4" width="100px;" alt=""/><br /><sub><b>Simon Jespersen</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=simjes" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/pulls?q=is%3Apr+reviewed-by%3Asimjes" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="https://afontcu.dev"><img src="https://avatars0.githubusercontent.com/u/9197791?v=4" width="100px;" alt=""/><br /><sub><b>Adrià Fontcuberta</b></sub></a><br /><a href="#infra-afontcu" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=afontcu" title="Documentation">📖</a> <a href="https://github.com/testing-library/cypress-testing-library/pulls?q=is%3Apr+reviewed-by%3Aafontcu" title="Reviewed Pull Requests">👀</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/Megoos"><img src="https://avatars2.githubusercontent.com/u/9866017?v=4" width="100px;" alt=""/><br /><sub><b>Mikhail Guskov</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/issues?q=author%3AMegoos" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://jds.work"><img src="https://avatars1.githubusercontent.com/u/10285055?v=4" width="100px;" alt=""/><br /><sub><b>JD Gonzales</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=juliusdelta" title="Documentation">📖</a></td>
    <td align="center"><a href="https://yvonnickfrin.dev"><img src="https://avatars0.githubusercontent.com/u/13099512?v=4" width="100px;" alt=""/><br /><sub><b>Yvonnick FRIN</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=frinyvonnick" title="Documentation">📖</a></td>
    <td align="center"><a href="https://www.franck-abgrall.me/"><img src="https://avatars3.githubusercontent.com/u/9840435?v=4" width="100px;" alt=""/><br /><sub><b>Franck Abgrall</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/pulls?q=is%3Apr+reviewed-by%3Akefranabg" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="http://twitter.com/tlrobinson"><img src="https://avatars0.githubusercontent.com/u/18193?v=4" width="100px;" alt=""/><br /><sub><b>Tom Robinson</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=tlrobinson" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=tlrobinson" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/NicholasBoll"><img src="https://avatars2.githubusercontent.com/u/338257?v=4" width="100px;" alt=""/><br /><sub><b>Nicholas Boll</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=NicholasBoll" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=NicholasBoll" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://github.com/FlopieUtd"><img src="https://avatars3.githubusercontent.com/u/23555863?v=4" width="100px;" alt=""/><br /><sub><b>FlopieUtd</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=FlopieUtd" title="Documentation">📖</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/leosuncin"><img src="https://avatars1.githubusercontent.com/u/4307697?v=4" width="100px;" alt=""/><br /><sub><b>Jaime Leonardo Suncin Cruz</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/issues?q=author%3Aleosuncin" title="Bug reports">🐛</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=leosuncin" title="Code">💻</a> <a href="https://github.com/testing-library/cypress-testing-library/commits?author=leosuncin" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://matt.travi.org"><img src="https://avatars1.githubusercontent.com/u/126441?v=4" width="100px;" alt=""/><br /><sub><b>Matt Travi</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=travi" title="Code">💻</a></td>
    <td align="center"><a href="https://michaeldeboey.be"><img src="https://avatars3.githubusercontent.com/u/6643991?v=4" width="100px;" alt=""/><br /><sub><b>Michaël De Boey</b></sub></a><br /><a href="https://github.com/testing-library/cypress-testing-library/commits?author=MichaelDeBoey" title="Code">💻</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors][all-contributors] specification.
Contributions of any kind welcome!

## LICENSE

MIT

[npm]: https://www.npmjs.com/
[node]: https://nodejs.org
[build-badge]:
  https://img.shields.io/travis/testing-library/cypress-testing-library.svg?style=flat-square
[build]: https://travis-ci.org/testing-library/cypress-testing-library
[coverage-badge]:
  https://img.shields.io/codecov/c/github/testing-library/cypress-testing-library.svg?style=flat-square
[coverage]: https://codecov.io/github/testing-library/cypress-testing-library
[version-badge]:
  https://img.shields.io/npm/v/cypress-testing-library.svg?style=flat-square
[package]: https://www.npmjs.com/package/@testing-library/cypress
[downloads-badge]:
  https://img.shields.io/npm/dm/@testing-library/cypress.svg?style=flat-square
[npmtrends]: http://www.npmtrends.com/@testing-library/cypress
[license-badge]:
  https://img.shields.io/npm/l/@testing-library/cypress.svg?style=flat-square
[license]:
  https://github.com/testing-library/cypress-testing-library/blob/master/LICENSE
[prs-badge]:
  https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square
[prs]: http://makeapullrequest.com
[coc-badge]:
  https://img.shields.io/badge/code%20of-conduct-ff69b4.svg?style=flat-square
[coc]:
  https://github.com/testing-library/cypress-testing-library/blob/master/other/CODE_OF_CONDUCT.md
[github-watch-badge]:
  https://img.shields.io/github/watchers/testing-library/cypress-testing-library.svg?style=social
[github-watch]:
  https://github.com/testing-library/cypress-testing-library/watchers
[github-star-badge]:
  https://img.shields.io/github/stars/testing-library/cypress-testing-library.svg?style=social
[github-star]:
  https://github.com/testing-library/cypress-testing-library/stargazers
[twitter]:
  https://twitter.com/intent/tweet?text=Check%20out%20cypress-testing-library%20by%20%40kentcdodds%20https%3A%2F%2Fgithub.com%2Fkentcdodds%2Fcypress-testing-library%20%F0%9F%91%8D
[twitter-badge]:
  https://img.shields.io/twitter/url/https/github.com/testing-library/cypress-testing-library.svg?style=social
[emojis]: https://github.com/kentcdodds/all-contributors#emoji-key
[all-contributors]: https://github.com/all-contributors/all-contributors
[dom-testing-library]: https://github.com/testing-library/dom-testing-library
[cypress]: https://www.cypress.io/
