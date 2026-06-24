# sbi-feedback

A Claude Code skill that generates structured positive feedback for work reports using the SBI (Situation, Behavior, Impact) framework.

## Usage

```
/sbi-feedback
```

Provide a work report as input. The skill returns a formatted SBI feedback message.

## Rules

- Strictly follows the SBI (Situation, Behavior, Impact) framework
- Uses a supportive tone that balances technical accuracy with personal encouragement
- Structures feedback with three clear sections: **S**ituation, **B**ehavior, **I**mpact
- Clarifies achievements, provides constructive insights, and promotes professional growth
- Includes specific, measurable observations and an encouraging closing message
- Uses professional yet warm language that celebrates individual contributions and potential

## Output Format

```
## SBI Type Feedback {YYYY-MM-DD GGG}

Situation (S): {Situation catchphrase}
{Explanation of current context in project progress}

Behavior (B): {Behavior catchphrase}
{Explanation of specific initiatives, innovations, and challenges undertaken}

Impact (I): {Impact catchphrase}
{Explanation of positive results and organizational contributions from this behavior}

### Noteworthy Points

{List of particularly excellent aspects or remarkable achievements in the work report.
 Example: Outstanding problem-solving approach. Efficient time management. Active team contributions.}

### Encouragement for Growth

{Message with encouragement toward the next steps}

### Experience Points Gained

{Quantified project skills and personal growth — multiple entries acceptable.
 Example: Project Management: 15XP, Technical Skills: 10XP, Communication: 8XP}
```
