# Starving Artist Jekyll Theme
![Starving Artist Jekyll Theme screenshot](screenshot.png)

# Install as Gem Theme
Jekyll requires Ruby so make sure Ruby is installed before you begin.

### Start a New Site
- Install Jekyll and Bundler
  - `gem install bundler`
- Create a New Site
  - `jekyl new mysite`
- Move into that directory
  - `cd mysite`
- Install Required Gems
  - `bundle install`
- Verify
  - Run `jekyll serve`
  - Browse to [http://localhost:4000](http://localhost:4000)
- Download Starving Artist Theme
  - Replace the line `gem "minima"` with this:
    - `gem "starving-artist-jekyll-theme"`
  - Run `bundle install`
- Tell Jekyll to use Starving Artist Theme
  - Open `_config.yml` and change the line `theme: minima` to this:
    - `theme: starving-artist-jekyll-theme`
- Import the Starving Artist CSS
  - Open your `css/style.css` and change the line `@import "minima;"` to this:
    - `@import "starving-artist";`


### Migrate an Existing Site
**NOTE** This requires you to be upgraded to at least Jekyll 3.2 which added support for themes.

- Download Starving Artist Theme
  - Replace the line `gem "minima"` with this:
    - `gem "starving-artist-jekyll-theme"`
  - Run `bundle install`
- Tell Jekyll to use Starving Artist Theme
  - Open `_config.yml` and change the line `theme: minima` to this:
    - `theme: starving-artist-jekyll-theme`
- Import the Starving Artist CSS
  - Open your `css/style.css` and change the line `@import "minima;"` to this:
    - `@import "starving-artist";`

# Jekyll 2.x Method
Jekyll requires Ruby so make sure Ruby is installed before you begin.

- Download this site
  - `git clone https://github.com/chrisanthropic/starving-artist-jekyll-theme.git`
- Move into its directory
  - `cd starving-artist-jekyll-theme`
- Install Required Gems
  - `bundle install`
- Verify
  - Run `jekyll serve`
  - Browse to [http://localhost:4000](http://localhost:4000)
