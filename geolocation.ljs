    document.addEventListener("DOMContentLoaded", function () {
      var allowedZips = {
        "geo-local": window.geoLocalZips || [],
        "geo-zones-2-3": window.geoZones2_3Zips || [],
        "geo-zone-4": window.geoZone4Zips || [],
        "geo-zones-5-8": window.geoZones5_8Zips || []
      };

  var multiColumnSections = {
    "geo-local": "shopify-section-template--24466623889686__multi_column_main_local",
    "geo-zones-2-3": "shopify-section-template--24466623889686__multi_column_main_zone_2_3",
    "geo-zone-4": "shopify-section-template--24466623889686__multi_column_main_zone_4",
    "geo-zones-5-8": "shopify-section-template--24466623889686__multi_column_main-zones_5_8"
  };

  var zipCode = localStorage.getItem("user_zipcode");
  var zipPopup = document.getElementById("zip-popup");
  var zipInput = document.getElementById("zip-input");
  var zipSubmit = document.getElementById("zip-submit");
  var zipAnnouncement = document.getElementById("zip-announcement");

  function setZipCode(zip) {
    localStorage.setItem("user_zipcode", zip);
    updateAnnouncementBar(zip);
    filterNavigationByZip(zip);
    filterProductsByZip(zip);
    showMultiColumnByZip(zip);
    redirectBasedOnZip(zip);
  }

  function updateAnnouncementBar(zip) {
    zipAnnouncement.innerHTML = zip + ' <a href="#" id="change-zip">Change Zipcode</a>';
  }

  function askForZipCode(prefillZip = "") {
    if (prefillZip) {
      zipInput.value = prefillZip;
    }
    zipPopup.style.display = "block";
  }

  function tryGeolocationPrefill(callback) {
    fetch("https://ipapi.co/json/")
      .then(response => response.json())
      .then(data => {
        if (data && data.postal) {
          const zip = data.postal.trim();
          callback(zip);
        } else {
          callback("");
        }
      })
      .catch(() => {
        callback("");
      });
  }

  if (!zipCode) {
    tryGeolocationPrefill(function (geoZip) {
      if (geoZip) {
        setZipCode(geoZip); 
        askForZipCode(geoZip);
      } else {
        askForZipCode();
      }
    });
  } else {
    updateAnnouncementBar(zipCode);
    filterNavigationByZip(zipCode);
    filterProductsByZip(zipCode);
    showMultiColumnByZip(zipCode);
    redirectBasedOnZip(zipCode);
  }

  zipSubmit.addEventListener("click", function () {
    var userZip = zipInput.value.trim();
    if (userZip) {
      zipPopup.style.display = "none";
      setZipCode(userZip);
    }
  });

  document.addEventListener("click", function (e) {
    if (e.target && e.target.id === "change-zip") {
      askForZipCode(localStorage.getItem("user_zipcode") || "");
    }
  });

  function filterProductsByZip(userZip) {
    var productLinks = document.querySelectorAll("a[href*='/collections/']");
    productLinks.forEach(function(link) {
      var href = link.getAttribute("href");
      var regex = /\/collections\/([^\/]+)\/products\//;
      var match = href.match(regex);
      if (match) {
        var collection = match[1];
        if (allowedZips[collection]) {
          if (allowedZips[collection].indexOf(userZip) === -1) {
            var productItem = link.closest(".product-item") || link.closest(".grid__item");
            if (productItem) {
              productItem.style.display = "none";
            } else {
              link.style.display = "none";
            }
          }
        }
      }
    });
  }

  function filterNavigationByZip(userZip) {
    var navMapping = {
      "header_local": "geo-local",
      "header_zone4": "geo-zone-4",
      "header_zones2_3": "geo-zones-2-3",
      "header_zones5_8": "geo-zones-5-8"
    };

    Object.keys(navMapping).forEach(function(navKey) {
      var zoneKey = navMapping[navKey];
      var navElements = document.querySelectorAll('[id$="__' + navKey + '"]');
      navElements.forEach(function(el) {
        if (allowedZips[zoneKey] && allowedZips[zoneKey].indexOf(userZip) === -1) {
          el.style.display = "none";
        } else {
          el.style.display = "block";
        }
        el.style.visibility = "visible";
      });
    });
  }

  function showMultiColumnByZip(userZip) {
    Object.keys(multiColumnSections).forEach(function(zoneKey) {
      var sectionId = multiColumnSections[zoneKey];
      var section = document.getElementById(sectionId);
      if (section) {
        if (allowedZips[zoneKey] && allowedZips[zoneKey].indexOf(userZip) === -1) {
          section.style.display = "none";
        } else {
          section.style.display = "block";
        }
      }
    });
  }

  function redirectBasedOnZip(userZip) {
    // Remove any trailing slash from the path
    var currentPath = window.location.pathname.replace(/\/$/, '');
    if (currentPath === '/pages/cleanse-programs') {
      if (allowedZips["geo-local"].indexOf(userZip) !== -1) {
        // User is in geo-local—stay on this page.
        return;
      } else if (allowedZips["geo-zone-4"].indexOf(userZip) !== -1) {
        window.location.href = "https://hi-vibe.com/pages/cleanse-programs-4";
      } else if (allowedZips["geo-zones-2-3"].indexOf(userZip) !== -1) {
        window.location.href = "https://hi-vibe.com/pages/cleanse-programs-2-3";
      } else if (allowedZips["geo-zones-5-8"].indexOf(userZip) !== -1) {
        window.location.href = "https://hi-vibe.com/pages/cleanse-programs-5-8";
      }
    }
  }
});
