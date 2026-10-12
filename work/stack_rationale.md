Portfolio Stack Decision Rationale
My Chosen Stack
I am building my portfolio using pure HTML and Tailwind CSS, hosted entirely on GitHub Pages.

Alternatives Considered
I evaluated going simpler with a Markdown-based generator like Jekyll, and going much heavier with a modern JavaScript framework like Next.js hosted on Vercel.

Why I Made This Choice (and Trade-offs)
My primary constraint is showcasing data engineering and machine learning work—specifically high-resolution screenshots of Jupyter execution, data leakage traps, and pipeline architecture. The site does not require a backend or dynamic data loading; it is strictly a static presentation layer.

I rejected Next.js because it is severe overkill. Building a full React application for a static portfolio introduces unnecessary maintenance overhead (dependency updates, package management) that distracts from my core focus on AI and data science. I rejected Jekyll because, while easy to maintain, it is visually rigid and difficult to customize to my specific identity kit (Inter font, Slate background, Sky Blue accent).

HTML with Tailwind CSS is the perfect middle ground. It allows me to exactly match my design constraints, structure my image galleries and code blocks precisely how I want them, and maintain the site indefinitely with zero risk of framework obsolescence. It perfectly frames my work without becoming a project itself.
