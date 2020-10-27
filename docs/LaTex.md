# LaTex

A Simple template:

```latex
\documentclass{article}

\usepackage{graphicx}
\usepackage{CJKutf8}
\usepackage{indentfirst}
\setlength{\parindent}{2em}
\usepackage{geometry}
\usepackage{gensymb}
\usepackage{enumerate}
\usepackage{float}
\usepackage{longtable}
\newcommand{\RNum}[1]{\uppercase\expandafter{\romannumeral #1\relax}}
\geometry{a4paper, scale = 0.8}
\begin{document}
\begin{CJK}{UTF8}{gbsn}

\title{Tile}
\author{Colin}
\maketitle


% Table
%\begin{table}[H]
%\setlength{\tabcolsep}{3mm}{
%\renewcommand\arraystretch{2}
%\begin{tabular}{|c|c|c|c|c|c|c|c|c|c|c|c|}
%\hline
%
%\end{tabular}
%}
%\end{table}

% Figure
%\begin{figure}[H]
%	\centering
%	\includegraphics[scale = 0.2]{img/datarecord02}
%\end{figure}

\end{CJK}
\end{document}
```