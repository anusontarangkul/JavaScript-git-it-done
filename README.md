# Git-it-Done

This app uses the GitHub API to search for open source projects. You can search by username or languages. Once you make a search, the app makes a fetch call to the github api. Then it formats the response in JSON and dynamically populatues repositories to click on. You can see if there are any issues for each repository.

![gif](./assets/gif/gif.gif)

|                                         |                                                               |                                                   |
| :-------------------------------------: | :-----------------------------------------------------------: | :-----------------------------------------------: |
|      [Introduction](#git-it-done)       |            [Table of Contents](#table-of-contents)            | [Development Highlights](#development-highlights) |
|         [Deployment](#deployed)         | [Description of Page Building](#Description-of-Page-Building) |       [Code Hightlights](#code-highlights)        |
| [Technologies Used](#Technologies-Used) |                      [Credits](#Credits)                      |                [License](#License)                |

## Development Highlight

- Use GitHub API
- Format data using JSON
- Use error handling
- Dynamic DOM manipulation for external API call

## Deployment

[Deployment](https://anusontarangkul.github.io/git-it-done/)

This app is deployed using GitHub pages.

## Description of Page Building

- assets
  - css
  - gif
  - js
- .gitignore
- index.html
- LICENSE
- README.md
- single-repo.html

2 html files are 2 corresponding js fils are used. The CSS for this app was already provided.

## Code Highlights

Dynamically fetch repositories form github by entering their username. A fetch call is made to github and then the data is converted to json to be passed on to the next function of displaying the repos. Error handling is added by alerting the user with the error.

```JavaScript
var getUserRepos = function (user) {
    // format the github api url
    var apiUrl = "https://api.github.com/users/" + user + "/repos";

    // make a request to the url
    fetch(apiUrl).then(function (response) {
        if (response.ok) {
            response.json().then(function (data) {
                displayRepos(data, user);
            });
        } else {
            alert("Error: " + response.status);
        }
    })
        .catch(function (error) {
            alert("Unable to connect to GitHub")
        });
}
```

Dynamically creating elements based on the repository search and the searchTerm from the previous GetUserRepos function. We first check if there are any repos. Next we loop through repo length to create the corresponding elements of the repositories.

```JavaScript
var displayRepos = function (repos, searchTerm) {
    // check if api returned any repos
    if (repos.length === 0) {
        repoContainerEl.textContent = "No repositories found."
        return;
    }

    repoContainerEl.textContent = "";
    repoSearchTerm.textContent = searchTerm;

    // loop over repos
    for (var i = 0; i < repos.length; i++) {
        // format repo name
        var repoName = repos[i].owner.login + "/" + repos[i].name;

        // create a container for each repo
        var repoEl = document.createElement("a");
        repoEl.classList = "list-item flex-row justify-space-between align-center";
        repoEl.setAttribute("href", "./single-repo.html?repo=" + repoName);
        // create a span element to hold repository name
        var titleEl = document.createElement("span");
        titleEl.textContent = repoName;

        // create a status element
        var statusEl = document.createElement("span");
        statusEl.classList = "flex-row align-center";

        // check if current repo has issues or not
        if (repos[i].open_issues_count > 0) {
            statusEl.innerHTML = "<i class='fas fa-times status-icon icon-danger'></i>" + repos[i].open_issues_count + " issue(s)";
        } else {
            statusEl.innerHTML = "<i class='fas fa-check-square status-icon icon-success'></i>";
        }

        // append div to container
        repoEl.appendChild(titleEl);

        // append container to the dom
        repoContainerEl.appendChild(repoEl);

        // append span to container
        repoEl.appendChild(statusEl)
    }
}
```

## Technologies Used

### Languages

- [HTML](https://www.w3schools.com/html/)
- [JavaScript](https://www.javascript.com/)
- [CSS](https://www.w3schools.com/css/)

### Design Libraries

- [Font-Awesome](https://fontawesome.com/)
- [Google-Fonts](https://fonts.google.com/)

### REST APIs

- [GitHub](https://docs.github.com/en/rest)

## Credits

|                           |                                                                                                                                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **David Anusontarangkul** | [![Linkedin](https://i.stack.imgur.com/gVE0j.png) LinkedIn](https://www.linkedin.com/in/anusontarangkul/) [![GitHub](https://i.stack.imgur.com/tskMh.png) GitHub](https://github.com/anusontarangkul) |

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
