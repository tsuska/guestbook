/**
 * Get the current URL.
 *
 * @param {function(string)} callback - called when the URL of the current tab
 *   is found.
 */
function getCurrentTabUrl(callback) {
  // Query filter to be passed to chrome.tabs.query - see
  // https://developer.chrome.com/extensions/tabs#method-query
  var queryInfo = {
    active: true,
    currentWindow: true
  };

  chrome.tabs.query(queryInfo, function(tabs) {
    // chrome.tabs.query invokes the callback with a list of tabs that match the
    // query. When the popup is opened, there is certainly a window and at least
    // one tab, so we can safely assume that |tabs| is a non-empty array.
    // A window can only have one active tab at a time, so the array consists of
    // exactly one tab.
    var tab = tabs[0];

    // A tab is a plain object that provides information about the tab.
    // See https://developer.chrome.com/extensions/tabs#type-Tab
    var url = tab.url;

    // tab.url is only available if the "activeTab" permission is declared.
    // If you want to see the URL of other tabs (e.g. after removing active:true
    // from |queryInfo|), then the "tabs" permission is required to see their
    // "url" properties.
    console.assert(typeof url == 'string', 'tab.url should be a string');

    callback(url);
  });

  // Most methods of the Chrome extension APIs are asynchronous. This means that
  // you CANNOT do something like this:
  //
  // var url;
  // chrome.tabs.query(queryInfo, function(tabs) {
  //   url = tabs[0].url;
  // });
  // alert(url); // Shows "undefined", because chrome.tabs.query is async.
}

/**
 * @param {string} url - Current tab URL.
 * @param {function(string)} callback - Called when server respond. The callback gets response.
 * @param {function(string)} errorCallback - Called when something wrong. The callback gets a string that describes the failure reason.
 */
function getResponse(url, callback, errorCallback) {
    var x = new XMLHttpRequest();
    x.open("POST", "https://default-dot-ordinal-mote-98419.appspot.com/suska");
    x.setRequestHeader("Content-Type", "application/json");
    x.send(JSON.stringify({URL: url}));
    x.responseType = 'json';
    x.onload = function() {
        var response = x.response;
        if (!response) {
            errorCallback("No response");
            return;
        }
        callback(JSON.stringify(response));
    };
    x.onerror = function() {
        errorCallback("Network error.");
    };
}

function renderURL(url) {
  document.getElementById('url').textContent = url;
}

function renderResponse(response) {
  document.getElementById('response').textContent = response;
}

document.addEventListener('DOMContentLoaded', function() {
  getCurrentTabUrl(function(url) {
    // Put the image URL in Google search.
    renderURL(url);

    getResponse(url, function(response) {
        renderResponse(response);
    }, function (errorMessage) {
        renderResponse(errorMessage);
    });
  });
});
