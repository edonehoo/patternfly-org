---
id: Severity 
section: patterns
---

import CriticalRiskIcon from '@patternfly/react-icons/dist/esm/icons/critical-risk-icon';
import SeverityImportantIcon from '@patternfly/react-icons/dist/esm/icons/severity-important-icon';
import SeverityMinorIcon from '@patternfly/react-icons/dist/esm/icons/severity-minor-icon';
import SeverityModerateIcon from '@patternfly/react-icons/dist/esm/icons/severity-moderate-icon';
import SeverityNoneIcon from '@patternfly/react-icons/dist/esm/icons/severity-none-icon';
import SeverityUndefinedIcon from '@patternfly/react-icons/dist/esm/icons/severity-undefined-icon';
import { Icon } from '@patternfly/react-core';
import './severity.css';


When there is an issue or incident related to a source of data, it is important to communicate the **severity** of the situation, to help users to measure and understand the impact that the situation will have on business.

A well-defined severity communication system supports cross-product parity and consistency, and makes it easier for users to understand severity-related terminology and symbols. 

To support severity communication, PatternFly offers a series of icons that correspond to different severity levels:

| **Icon** |  **Color token** | **Severity** | **Usage** |
| --- | --- | --- | --- |
| <Icon iconSize="lg" className="critical"><CriticalRiskIcon /></Icon> | `--pf-t--global--icon--color--severity--critical--default`| Critical | |
| <Icon iconSize="lg" className="important"><SeverityImportantIcon /></Icon>  | `--pf-t--global--icon--color--severity--important--default`| Important | |
| <Icon iconSize="lg" className="moderate"><SeverityModerateIcon /></Icon>  | `--pf-t--global--icon--color--severity--moderate--default`| Moderate | |
| <Icon iconSize="lg" className="minor"><SeverityMinorIcon /></Icon>  | `--pf-t--global--icon--color--severity--minor--default`| Minor | |
| <Icon iconSize="lg" className="none"><SeverityNoneIcon /></Icon> | `--pf-t--global--icon--color--severity--none--default`| None | |
| <Icon iconSize="lg" className="undefined"><SeverityUndefinedIcon /></Icon>  | `--pf-t--global--icon--color--severity--undefined--default`| Undefined | |
