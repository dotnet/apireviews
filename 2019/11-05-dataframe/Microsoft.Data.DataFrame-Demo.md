# Demo

```C#
string filename = Path.Combine(FOLDER, "GitHubIssueDownloaderFormat.tsv");
DataFrame df = DataFrame.LoadCsv(filename, separator: '\t', header: true);
// Goal is to train an IssueLabeller
// Step1: Our GitHub data combines issues and PRs. I segment into two different DataFrames, one for PR, one for Issues
DataFrame prs = df.Filter(df["IsPR"].ElementwiseEquals(1));
DataFrame issues = df.Filter(df["IsPR"].ElementwiseEquals(0));

// For our model training, the "ID" and "FilePaths" do not always correlate well with our label "Area". How do I know? I found the Pearson correlation coefficient between the columns like so:
var corr = LinqStatistics.EnumerableStats.Pearson(df["FilePaths"].Cast<string>(), df["Area"].Cast<float>());

issues.Columns.Remove("ID");
issues.Columns.Remove("FilePaths");

// However, most issues/PRs mention users/owners. So, parse Descriptions, find user mentions and add it as a new column to issues
StringDataFrameColumn description = issues["Description"] as StringDataFrameColumn;
StringDataFrameColumn userMentionsColumn = new StringDataFrameColumn("UserMentions", issues.RowCount);
var regex = new Regex(@"@[a-zA-Z0-9_//-]+");
description.ApplyElementwise((string current, long rowIndex) =>
{
    var userMentions = regex.Matches(current).Select(x => x.Value).ToArray();
    userMentionsColumn[rowIndex] = string.Join(' ', userMentions);
    return current;
});
issues.Columns.Add(userMentionsColumn);

// Step2: The input to training is three files for each of the two: Train dataset (80%), Validate dataset (next 10%), test dataset (last 10%)
DataFrame trainIssues = SplitTrainTest(issues, 0.8f, out DataFrame tempValidateAndTest);
DataFrame validateIssues = SplitTrainTest(tempValidateAndTest, 0.5f, out DataFrame testIssues);

MLContext mLContext = new MLContext();

Train(mLContext, trainIssues, validateIssues, testIssues, IssueModelPath, IssueFittedModelPath, DeployIssueFittedModelPath);
```
