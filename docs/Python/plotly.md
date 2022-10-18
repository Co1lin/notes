# Plotly

## draw multiple lines

```python
def draw_figure_pair(
    XYs: Dict[str, List[List[float | int]]],
    title: str | None, xaxis_title: str, yaxis_title: str,
) -> go.Figure:
    fig = go.Figure()
    for name, (X, Y) in XYs.items():
        fig.add_trace(go.Scatter(
            x=X, y=Y, mode='lines', name=name,
        ))
    # end for
    fig.update_layout(
        title={
            'text': title,
            'x': 0.5,
        } if title else None,
        xaxis_title=xaxis_title,
        yaxis_title=yaxis_title,
        # margin=dict(l=0,r=0,b=0,t=0),
      	font=dict(size=16),
    )
    fig.update_layout(
        legend=dict(
            yanchor="top", y=0.99,
            xanchor="right", x=0.99,
        ),
    )
    # fig.update_xaxes(showgrid=False, showline=True, mirror=True, linewidth=1, linecolor='black')
    # fig.update_yaxes(showgrid=False, showline=True, mirror=True, linewidth=1, linecolor='black')
    # fig.update_layout({
    #     'plot_bgcolor': 'rgba(0, 0, 0, 0)',
    # })
    return fig

fig = draw_figure_pair(
    {
        'k = 4': [log_4['iteration'], log_4['best cost']],
        'k = 6': [log_6['iteration'], log_6['best cost']],
        'k = 8': [log_8['iteration'], log_8['best cost']],
    },
    'Best cost - iteration', 'iteration', 'best cost',
)
fig.show()
```

## legend group

```python
color_seq = px.colors.qualitative.Plotly
fig.add_traces([
  go.Scatter(
    x=X_inc, y=Y_inc, mode='lines',
    line=dict(color=color_seq[i_group], dash='solid'),
    legendgroup=f'group name', legendgrouptitle_text=f'group title',
    name=f'line name',
  ),
  go.Scatter(
    x=X_noinc, y=Y_noinc, mode='lines',
    line=dict(color=color_seq[i_group], dash='dot'),
    legendgroup=f'group name', legendgrouptitle_text=f'group title',
    name=f'line name',
  ),
])
```



