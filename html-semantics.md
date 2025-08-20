# HTML Semantics:

#### Table of Contents <a href="#pdf-page-8exucbr4qwlvgx57eq2x-table-of-contents" id="pdf-page-8exucbr4qwlvgx57eq2x-table-of-contents"></a>

1. [Foundation: What Are Semantics?](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#foundation)
2. [The Mental Model](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#mental-model)
3. [Document Structure Elements](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#document-structure)
4. [Content Sectioning Elements](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#content-sectioning)
5. [Text-Level Semantics](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#text-level)
6. [Interactive Elements](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#interactive-elements)
7. [Form Semantics](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#form-semantics)
8. [Media & Embedded Content](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#media-embedded)
9. [The Hidden Powers in Detail](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#hidden-powers)
10. [Decision Tree: Which Tag to Use?](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#decision-tree)
11. [Common Mistakes & Fixes](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#common-mistakes)
12. [Real-World Examples](https://claude.ai/chat/cfe37372-0758-4e38-aed8-c34e1d46a94d#real-world-examples)

***

#### Foundation: What Are Semantics? {#foundation} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-foundation-what-are-semantics-foundation" id="pdf-page-8exucbr4qwlvgx57eq2x-foundation-what-are-semantics-foundation"></a>

**Core Concept**: HTML semantics is about choosing tags based on **MEANING**, not appearance.

**The Box Analogy Extended**

Imagine you're organizing a massive warehouse:

**Non-Semantic Approach** (All boxes labeled "Box"):

Copy

```
<div>Product Name</div>
<div>Price: $99</div>
<div>Description: This product...</div>
<div>Add to Cart</div>
```

**Semantic Approach** (Properly labeled boxes):

Copy

```
<h2>Product Name</h2>
<data value="99">Price: $99</data>
<p>Description: This product...</p>
<button>Add to Cart</button>
```

**The Three Layers of Web Content**

1. **HTML (Structure & Meaning)** - The skeleton and labels
2. **CSS (Presentation)** - The skin and styling
3. **JavaScript (Behavior)** - The muscles and actions

Semantics ONLY concerns layer 1 - meaning and structure.

***

#### The Mental Model {#mental-model} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-the-mental-model-mental-model" id="pdf-page-8exucbr4qwlvgx57eq2x-the-mental-model-mental-model"></a>

**Think Like a Librarian**

When a librarian organizes books, they don't just put them anywhere. They create sections:

* Fiction → Romance → Historical Romance
* Non-Fiction → Science → Biology → Marine Biology

HTML semantics works the same way. Every piece of content has a **proper category**.

**The "Robot Test"**

Ask yourself: "If a robot read my HTML (without seeing the visual design), would it understand what each piece of content represents?"

**Robot reads non-semantic HTML**: "I see 8 generic containers with text" **Robot reads semantic HTML**: "I see a navigation menu, main article, sidebar with related links, and a footer"

***

#### Document Structure Elements {#document-structure} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-document-structure-elements-document-structure" id="pdf-page-8exucbr4qwlvgx57eq2x-document-structure-elements-document-structure"></a>

**The Big Five Container Elements**

**1. `<header>` - The Crown**

**When to use**: Top-level introductory content **Mental trigger**: "This introduces what comes after"

Copy

```
<!-- Page header -->
<header>
  <img src="logo.png" alt="Company Logo">
  <h1>TechBlog Daily</h1>
  <p>Your source for tech news</p>
</header>

<!-- Article header -->
<article>
  <header>
    <h2>iPhone 16 Review</h2>
    <p>By John Doe on March 15, 2025</p>
  </header>
  <p>Article content...</p>
</article>
```

**2. `<nav>` - The Roadmap**

**When to use**: Links that help users navigate **Mental trigger**: "This helps people get around"

Copy

```
<!-- Main site navigation -->
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>

<!-- Breadcrumb navigation -->
<nav aria-label="Breadcrumb">
  <a href="/">Home</a> > 
  <a href="/products">Products</a> > 
  <span>iPhone Cases</span>
</nav>

<!-- Table of contents -->
<nav aria-label="Table of contents">
  <h3>In this article:</h3>
  <ul>
    <li><a href="#intro">Introduction</a></li>
    <li><a href="#features">Features</a></li>
  </ul>
</nav>
```

**3. `<main>` - The Star of the Show**

**When to use**: The primary content (only ONE per page) **Mental trigger**: "This is why the user came here"

Copy

```
<main>
  <!-- Everything inside here is the main purpose of this page -->
  <h1>How to Learn JavaScript</h1>
  <p>JavaScript is a programming language...</p>
  
  <section>
    <h2>Getting Started</h2>
    <p>First, you need...</p>
  </section>
</main>
```

**4. `<aside>` - The Supporting Actor**

**When to use**: Content related but not essential **Mental trigger**: "This is extra info that supports the main content"

Copy

```
<main>
  <article>
    <h1>Climate Change Effects</h1>
    <p>Climate change is affecting...</p>
  </article>
  
  <aside>
    <h3>Related Articles</h3>
    <ul>
      <li><a href="/renewable-energy">Renewable Energy Guide</a></li>
      <li><a href="/carbon-footprint">Reduce Your Carbon Footprint</a></li>
    </ul>
  </aside>
</main>
```

**5. `<footer>` - The Closing Credits**

**When to use**: Concluding information **Mental trigger**: "This wraps things up"

Copy

```
<!-- Page footer -->
<footer>
  <p>&copy; 2025 My Company</p>
  <nav>
    <a href="/privacy">Privacy Policy</a>
    <a href="/terms">Terms</a>
  </nav>
</footer>

<!-- Article footer -->
<article>
  <h2>My Blog Post</h2>
  <p>Content here...</p>
  <footer>
    <p>Tags: HTML, CSS, JavaScript</p>
    <p>Share this post</p>
  </footer>
</article>
```

***

#### Content Sectioning Elements {#content-sectioning} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-content-sectioning-elements-content-sectioning" id="pdf-page-8exucbr4qwlvgx57eq2x-content-sectioning-elements-content-sectioning"></a>

**`<section>` - The Chapter**

**When to use**: Thematic grouping of content with a heading **Mental trigger**: "This could be a chapter in a book"

Copy

```
<main>
  <h1>Complete Guide to Photography</h1>
  
  <section>
    <h2>Camera Basics</h2>
    <p>Understanding aperture, shutter speed...</p>
  </section>
  
  <section>
    <h2>Composition Techniques</h2>
    <p>Rule of thirds, leading lines...</p>
  </section>
  
  <section>
    <h2>Post-Processing</h2>
    <p>Editing your photos...</p>
  </section>
</main>
```

**`<article>` - The Standalone Piece**

**When to use**: Content that makes sense by itself **Mental trigger**: "Could this be published somewhere else and still make sense?"

Copy

```
<!-- Blog post -->
<article>
  <header>
    <h2>10 Tips for Better Sleep</h2>
    <time datetime="2025-03-15">March 15, 2025</time>
  </header>
  <p>Getting quality sleep is essential...</p>
  <footer>
    <p>Author: Dr. Sarah Johnson</p>
  </footer>
</article>

<!-- Product listing -->
<article>
  <h3>MacBook Pro 16"</h3>
  <img src="macbook.jpg" alt="MacBook Pro">
  <p>Powerful laptop for professionals</p>
  <data value="2499">$2,499</data>
</article>
```

**`<section>` vs `<article>` vs `<div>` - The Decision Matrix**

ElementWhen to UseExample

`<article>`

Standalone, redistributable content

Blog post, news article, product card

`<section>`

Thematic sections within a document

Chapters, tabs, accordion panels

`<div>`

Styling/layout only, no semantic meaning

Wrapper for CSS grid, containers

***

#### Text-Level Semantics {#text-level} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-text-level-semantics-text-level" id="pdf-page-8exucbr4qwlvgx57eq2x-text-level-semantics-text-level"></a>

**Emphasis and Importance**

**`<strong>` vs `<em>` vs `<b>` vs `<i>`**

**Theory**: Choose based on meaning, not appearance

Copy

```
<!-- IMPORTANCE (strong) -->
<p><strong>Warning:</strong> This action cannot be undone.</p>
<p>The <strong>most important thing</strong> to remember is...</p>

<!-- EMPHASIS (em) -->
<p>I <em>really</em> think you should consider this option.</p>
<p>The word <em>semantics</em> comes from Greek.</p>

<!-- STYLISTIC (b and i - avoid these, use CSS instead) -->
<p><b>Bold text</b> - only for visual styling</p>
<p><i>Italic text</i> - only for visual styling</p>
```

**Specific Content Types**

**`<time>` - Dates and Times**

**Power**: Screen readers can announce dates properly, search engines understand timing

Copy

```
<!-- Basic date -->
<time datetime="2025-03-15">March 15, 2025</time>

<!-- Date with time -->
<time datetime="2025-03-15T14:30:00">March 15, 2025 at 2:30 PM</time>

<!-- Relative time -->
<time datetime="2025-03-15" title="March 15, 2025">Yesterday</time>

<!-- Duration -->
<p>The movie is <time datetime="PT2H30M">2 hours and 30 minutes</time> long.</p>
```

**`<address>` - Contact Information**

**When to use**: Only for contact info of the article/page author

Copy

```
<article>
  <h1>My Latest Blog Post</h1>
  <p>Content here...</p>
  
  <address>
    <p>Written by <a href="mailto:jane@example.com">Jane Smith</a></p>
    <p>For questions, call <a href="tel:+1234567890">123-456-7890</a></p>
  </address>
</article>
```

**`<abbr>` - Abbreviations and Acronyms**

**Power**: Screen readers can expand abbreviations

Copy

```
<p>The <abbr title="World Health Organization">WHO</abbr> announced new guidelines.</p>
<p>We use <abbr title="HyperText Markup Language">HTML</abbr> for web structure.</p>
```

**`<cite>` - References and Citations**

Copy

```
<p>As mentioned in <cite>The Art of Web Design</cite>, good semantics matter.</p>
<blockquote>
  <p>The web is for everyone.</p>
  <cite>Tim Berners-Lee</cite>
</blockquote>
```

***

#### Interactive Elements {#interactive-elements} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-interactive-elements-interactive-elements" id="pdf-page-8exucbr4qwlvgx57eq2x-interactive-elements-interactive-elements"></a>

**Button vs Link Decision Tree**

**Mental Model**:

* **Button** = Does something on THIS page (submit, toggle, calculate)
* **Link** = Takes you SOMEWHERE (different page, section, download)

Copy

```
<!-- BUTTONS (actions) -->
<button type="submit">Save Changes</button>
<button onclick="toggleMenu()">Menu</button>
<button type="button">Calculate Total</button>

<!-- LINKS (navigation) -->
<a href="/contact">Contact Us</a>
<a href="#section1">Jump to Section 1</a>
<a href="document.pdf" download>Download PDF</a>
```

**`<details>` and `<summary>` - Built-in Collapsible Content**

**Power**: No JavaScript needed for show/hide functionality

Copy

```
<details>
  <summary>What is semantic HTML?</summary>
  <p>Semantic HTML uses elements that describe the meaning of content, not just its appearance. This helps with accessibility, SEO, and maintainability.</p>
</details>

<details open>
  <summary>Frequently Asked Questions</summary>
  <details>
    <summary>How do I get started?</summary>
    <p>Begin by replacing your divs with semantic elements...</p>
  </details>
</details>
```

***

#### Form Semantics {#form-semantics} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-form-semantics-form-semantics" id="pdf-page-8exucbr4qwlvgx57eq2x-form-semantics-form-semantics"></a>

**The Power of Proper Form Semantics**

**Theory**: Forms are where semantics have the biggest impact on user experience

**`<fieldset>` and `<legend>` - Grouping Related Fields**

Copy

```
<form>
  <fieldset>
    <legend>Personal Information</legend>
    <label for="fname">First Name:</label>
    <input type="text" id="fname" name="fname" required>
    
    <label for="lname">Last Name:</label>
    <input type="text" id="lname" name="lname" required>
  </fieldset>
  
  <fieldset>
    <legend>Contact Preferences</legend>
    <input type="radio" id="email" name="contact" value="email">
    <label for="email">Email</label>
    
    <input type="radio" id="phone" name="contact" value="phone">
    <label for="phone">Phone</label>
  </fieldset>
</form>
```

**Input Types - Be Specific**

**Power**: Mobile keyboards adapt, browsers provide validation

Copy

```
<!-- Email keyboard on mobile -->
<input type="email" placeholder="user@example.com">

<!-- Numeric keypad on mobile -->
<input type="tel" placeholder="123-456-7890">

<!-- Date picker -->
<input type="date" min="2025-01-01" max="2025-12-31">

<!-- Range slider -->
<input type="range" min="0" max="100" value="50">

<!-- Color picker -->
<input type="color" value="#ff0000">
```

***

#### Media & Embedded Content {#media-embedded} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-media-and-embedded-content-media-embedded" id="pdf-page-8exucbr4qwlvgx57eq2x-media-and-embedded-content-media-embedded"></a>

**`<figure>` and `<figcaption>` - Self-Contained Content**

**When to use**: Images, diagrams, code blocks that are referenced in text

Copy

```
<article>
  <p>The new design follows modern principles, as shown in Figure 1.</p>
  
  <figure>
    <img src="design-mockup.png" alt="Website mockup showing clean, minimal layout">
    <figcaption>Figure 1: The new homepage design featuring improved navigation and content hierarchy</figcaption>
  </figure>
  
  <p>As we can see from the figure above...</p>
</article>
```

**`<picture>` - Responsive Images with Meaning**

Copy

```
<picture>
  <source media="(min-width: 800px)" srcset="large-image.jpg">
  <source media="(min-width: 400px)" srcset="medium-image.jpg">
  <img src="small-image.jpg" alt="Sunset over mountains">
</picture>
```

***

#### The Hidden Powers in Detail {#hidden-powers} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-the-hidden-powers-in-detail-hidden-powers" id="pdf-page-8exucbr4qwlvgx57eq2x-the-hidden-powers-in-detail-hidden-powers"></a>

**Power 1: Screen Reader Navigation Superpowers**

**What happens with good semantics**:

Copy

```
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>

<main>
  <article>
    <h1>Article Title</h1>
    <p>Content...</p>
  </article>
</main>
```

**Screen reader user experience**:

* Press "R" → Jump to regions (nav, main, aside)
* Press "H" → Jump between headings (h1, h2, h3)
* Press "N" → Jump to next navigation
* Press "B" → Jump to next button

**Without semantics**: User hears "link, link, text, text, text" with no context.

**Power 2: SEO and Search Understanding**

**Google's Understanding**:

Copy

```
<!-- Google thinks: "This is a news article about technology, published recently" -->
<article>
  <header>
    <h1>Apple Announces New iPhone</h1>
    <time datetime="2025-03-15">March 15, 2025</time>
  </header>
  <p>Apple today announced...</p>
</article>

<!-- Google thinks: "This is navigation, not main content" -->
<nav>
  <a href="/phones">Phones</a>
  <a href="/tablets">Tablets</a>
</nav>
```

**SEO Benefits**:

* Rich snippets in search results
* Featured snippets (answer boxes)
* Better content categorization
* Enhanced search result displays

**Power 3: Browser Superpowers**

**Reading Mode**

Browsers like Safari can automatically detect article content:

Copy

```
<!-- Browser reading mode FINDS this content -->
<main>
  <article>
    <h1>How to Cook Pasta</h1>
    <p>Cooking perfect pasta requires...</p>
  </article>
</main>

<!-- Browser reading mode IGNORES this -->
<nav>Navigation links</nav>
<aside>Advertisements</aside>
```

**Automatic Outline Generation**

Copy

```
<h1>Complete Guide to Web Development</h1>
  <h2>Frontend Development</h2>
    <h3>HTML Basics</h3>
    <h3>CSS Styling</h3>
  <h2>Backend Development</h2>
    <h3>Server Setup</h3>
    <h3>Database Design</h3>
```

Browser creates clickable outline automatically.

**Power 4: Voice Assistant Integration**

Copy

```
<!-- Voice assistants understand this structure -->
<article>
  <h1>Weather Update</h1>
  <p>Today's temperature will reach <data value="25">25°C</data></p>
  <time datetime="2025-03-15">March 15, 2025</time>
</article>
```

***

#### Decision Tree: Which Tag to Use? {#decision-tree} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-decision-tree-which-tag-to-use-decision-tree" id="pdf-page-8exucbr4qwlvgx57eq2x-decision-tree-which-tag-to-use-decision-tree"></a>

**The Ultimate Decision Flowchart**

**Is it navigation links?** → `<nav>`

**Is it the main content of the page?** → `<main>`

**Is it a complete, standalone piece?** → `<article>`

* Blog post ✓
* News article ✓
* Product listing ✓
* Comment ✓
* Social media post ✓

**Is it a thematic section with a heading?** → `<section>`

* Chapter in a guide ✓
* Tab panel ✓
* Part of a longer article ✓

**Is it supplementary information?** → `<aside>`

* Sidebar ✓
* Related links ✓
* Author bio ✓
* Advertisements ✓

**Is it just for styling/layout?** → `<div>`

**Heading Hierarchy Rules**

**Think like a book outline**:

Copy

```
<h1>Book Title</h1>              <!-- Only one per page -->
  <h2>Chapter 1</h2>             <!-- Major sections -->
    <h3>Section 1.1</h3>         <!-- Subsections -->
      <h4>Subsection 1.1.1</h4>  <!-- Sub-subsections -->
        <h5>Detail point</h5>     <!-- Rarely needed -->
          <h6>Fine detail</h6>    <!-- Almost never needed -->
```

**Never skip levels**: Don't go from h1 to h3. Always use the next level down.

***

#### Common Mistakes & Fixes {#common-mistakes} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-common-mistakes-and-fixes-common-mistakes" id="pdf-page-8exucbr4qwlvgx57eq2x-common-mistakes-and-fixes-common-mistakes"></a>

**Mistake 1: Using Headings for Styling**

Copy

```
<!-- WRONG: Using h3 because it "looks right" -->
<h1>Main Title</h1>
<h3>Should be h2 but h3 looks better</h3>

<!-- RIGHT: Use proper hierarchy, style with CSS -->
<h1>Main Title</h1>
<h2 class="smaller-heading">Properly structured heading</h2>
```

**Mistake 2: Div Soup**

Copy

```
<!-- WRONG: Everything is a div -->
<div class="header">
  <div class="title">My Blog</div>
  <div class="nav">
    <div class="nav-item">Home</div>
    <div class="nav-item">About</div>
  </div>
</div>

<!-- RIGHT: Semantic elements -->
<header>
  <h1>My Blog</h1>
  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>
```

**Mistake 3: Multiple `<main>` Elements**

Copy

```
<!-- WRONG: Only one main per page -->
<main>Content 1</main>
<main>Content 2</main>

<!-- RIGHT: One main, multiple sections -->
<main>
  <section>Content 1</section>
  <section>Content 2</section>
</main>
```

**Mistake 4: Semantic Elements for Styling**

Copy

```
<!-- WRONG: Using blockquote for indentation -->
<blockquote>
  <p>This isn't actually a quote, I just want it indented</p>
</blockquote>

<!-- RIGHT: Use CSS for styling -->
<p class="indented">This needs special styling</p>
```

***

#### Real-World Examples {#real-world-examples} <a href="#pdf-page-8exucbr4qwlvgx57eq2x-real-world-examples-real-world-examples" id="pdf-page-8exucbr4qwlvgx57eq2x-real-world-examples-real-world-examples"></a>

**Example 1: Blog Post Page**

Copy

```
<!DOCTYPE html>
<html lang="en">
<head>
  <title>How to Learn HTML - TechBlog</title>
</head>
<body>
  <header>
    <img src="logo.png" alt="TechBlog Logo">
    <h1>TechBlog</h1>
  </header>
  
  <nav aria-label="Main navigation">
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/tutorials">Tutorials</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
  
  <main>
    <article>
      <header>
        <h1>How to Learn HTML in 30 Days</h1>
        <p>Published on <time datetime="2025-03-15">March 15, 2025</time></p>
        <p>By <address><a href="mailto:jane@techblog.com">Jane Doe</a></address></p>
      </header>
      
      <section>
        <h2>Week 1: Basics</h2>
        <p>Start with understanding what HTML actually is...</p>
        
        <h3>Day 1-3: Structure</h3>
        <p>Learn about document structure...</p>
      </section>
      
      <section>
        <h2>Week 2: Semantics</h2>
        <p>This is where the magic happens...</p>
      </section>
      
      <footer>
        <p>Tags: 
          <a href="/tag/html">HTML</a>, 
          <a href="/tag/beginner">Beginner</a>
        </p>
      </footer>
    </article>
    
    <aside>
      <h3>Related Articles</h3>
      <article>
        <h4><a href="/css-basics">CSS Basics</a></h4>
        <p>Learn styling after HTML...</p>
      </article>
    </aside>
  </main>
  
  <footer>
    <p>&copy; 2025 TechBlog. All rights reserved.</p>
  </footer>
</body>
</html>
```

**Example 2: E-commerce Product Page**

Copy

```
<main>
  <article>
    <header>
      <h1>iPhone 15 Pro</h1>
      <p>Professional photography. Supercharged.</p>
    </header>
    
    <figure>
      <img src="iphone15pro.jpg" alt="iPhone 15 Pro in natural titanium">
      <figcaption>iPhone 15 Pro available in four titanium finishes</figcaption>
    </figure>
    
    <section>
      <h2>Price</h2>
      <p>Starting at <data value="999">$999</data></p>
    </section>
    
    <section>
      <h2>Key Features</h2>
      <ul>
        <li><strong>A17 Pro chip</strong> - Next-level performance</li>
        <li><strong>Pro camera system</strong> - 48MP main camera</li>
      </ul>
    </section>
    
    <aside>
      <h3>Technical Specifications</h3>
      <dl>
        <dt>Display</dt>
        <dd>6.1-inch Super Retina XDR</dd>
        
        <dt>Storage</dt>
        <dd>128GB, 256GB, 512GB, 1TB</dd>
      </dl>
    </aside>
  </article>
</main>
```

**Example 3: News Website Layout**

Copy

```
<header>
  <h1>Daily News</h1>
  <nav aria-label="Main sections">
    <a href="/world">World</a>
    <a href="/tech">Technology</a>
    <a href="/sports">Sports</a>
  </nav>
</header>

<main>
  <section aria-labelledby="breaking-news">
    <h2 id="breaking-news">Breaking News</h2>
    
    <article>
      <header>
        <h3>Major Tech Announcement</h3>
        <time datetime="2025-03-15T10:30:00">10:30 AM</time>
      </header>
      <p>Summary of breaking news...</p>
    </article>
  </section>
  
  <section aria-labelledby="featured-stories">
    <h2 id="featured-stories">Featured Stories</h2>
    
    <article>
      <h3>Climate Change Summit Results</h3>
      <p>World leaders agreed on...</p>
      <footer>
        <p>Reporter: <cite>John Smith</cite></p>
      </footer>
    </article>
    
    <article>
      <h3>New Space Mission Launch</h3>
      <p>NASA successfully launched...</p>
    </article>
  </section>
</main>

<aside>
  <h2>Weather</h2>
  <p>Current temperature: <data value="22">22°C</data></p>
  
  <h2>Popular Articles</h2>
  <nav aria-label="Popular articles">
    <a href="/article1">Most read story</a>
    <a href="/article2">Trending topic</a>
  </nav>
</aside>
```

***

#### Advanced Semantic Patterns <a href="#pdf-page-8exucbr4qwlvgx57eq2x-advanced-semantic-patterns" id="pdf-page-8exucbr4qwlvgx57eq2x-advanced-semantic-patterns"></a>

**Microdata Integration**

**Power**: Even richer search results

Copy

```
<article itemscope itemtype="http://schema.org/Recipe">
  <header>
    <h1 itemprop="name">Chocolate Chip Cookies</h1>
    <p>Prep time: <time itemprop="prepTime" datetime="PT15M">15 minutes</time></p>
  </header>
  
  <section>
    <h2>Ingredients</h2>
    <ul>
      <li itemprop="recipeIngredient">2 cups flour</li>
      <li itemprop="recipeIngredient">1 cup sugar</li>
    </ul>
  </section>
</article>
```

**ARIA Labels for Enhanced Semantics**

Copy

```
<nav aria-label="User account navigation">
  <a href="/profile">Profile</a>
  <a href="/settings">Settings</a>
  <a href="/logout">Logout</a>
</nav>

<section aria-labelledby="recent-posts">
  <h2 id="recent-posts">Recent Posts</h2>
  <!-- articles here -->
</section>
```

***

#### Quick Reference Cheat Sheet <a href="#pdf-page-8exucbr4qwlvgx57eq2x-quick-reference-cheat-sheet" id="pdf-page-8exucbr4qwlvgx57eq2x-quick-reference-cheat-sheet"></a>

**Essential Elements by Purpose**

**Page Structure**:

* `<header>` - Introductory content
* `<nav>` - Navigation links
* `<main>` - Primary content (one per page)
* `<aside>` - Sidebar/supplementary content
* `<footer>` - Concluding content

**Content Organization**:

* `<article>` - Standalone, complete content
* `<section>` - Thematic grouping with heading
* `<h1>-<h6>` - Hierarchical headings

**Text Meaning**:

* `<strong>` - Important (not just bold)
* `<em>` - Emphasized (not just italic)
* `<time>` - Dates and times
* `<abbr>` - Abbreviations
* `<cite>` - Citations and references

**Interactive**:

* `<button>` - Triggers action
* `<a>` - Links to other content
* `<details>/<summary>` - Collapsible content

**Forms**:

* `<fieldset>/<legend>` - Group related fields
* `<label>` - Describes form inputs
* Specific `input` types (email, tel, date, etc.)

**Media**:

* `<figure>/<figcaption>` - Images with captions
* `<picture>` - Responsive images

***

#### Master's Mental Model <a href="#pdf-page-8exucbr4qwlvgx57eq2x-masters-mental-model" id="pdf-page-8exucbr4qwlvgx57eq2x-masters-mental-model"></a>

**The Semantic Mindset Shift**

**Old thinking**: "What div do I need to make this look right?" **New thinking**: "What does this content represent, and what's the most accurate HTML element for it?"

**The Three Questions Method**

Before writing any HTML element, ask:

1. **"What is this content's PURPOSE?"** (navigation, main content, sidebar)
2. **"What is this content's ROLE?"** (heading, paragraph, list, button)
3. **"How does this relate to OTHER content?"** (part of article, standalone piece, supporting info)

**Recognition Patterns**

Train yourself to automatically recognize these patterns:

**See a list of links** → Think `<nav>` **See a blog post** → Think `<article>` **See a sidebar** → Think `<aside>` **See a heading** → Think proper `<h1>-<h6>` level **See a date** → Think `<time>` **See important text** → Think `<strong>` (not `<b>`) **See emphasized text** → Think `<em>` (not `<i>`)

***

#### The Semantic HTML Mastery Test <a href="#pdf-page-8exucbr4qwlvgx57eq2x-the-semantic-html-mastery-test" id="pdf-page-8exucbr4qwlvgx57eq2x-the-semantic-html-mastery-test"></a>

You've mastered semantics when you can look at any website and instantly know:

* Which elements should be `<article>` vs `<section>`
* Where `<aside>` content belongs
* How to structure headings hierarchically
* When to use `<button>` vs `<a>`
* How to make forms accessible with proper labeling

Remember: **Semantics is about meaning, not appearance.** Master this mindset, and you'll write HTML that works better for everyone - humans and machines alike.
