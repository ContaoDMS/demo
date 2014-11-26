<?php
/* parameters */
// max count of documents, set to 0 (zero) for an unlimited list
$maxDocuments = 10;
// max length of document name, set to 0 (zero) for complete name
$maxDocNameLength = 0;
?>

<style type="text/css">
.newIcon {display: none; color: #FFFF00; cursor: help; font-size: 18px; font-weight: bold; line-height: 9px; margin-left: 2px; position: relative; text-shadow: 1px 0 3px #000000; top: 4px;}
.new .newIcon {display: inline;}
</style>

<div class="mod_dms <?php echo $this->class; ?> block"<?php echo $this->cssID; ?><?php if ($this->style): ?> style="<?php echo $this->style; ?>"<?php endif; ?>>
<?php if ($this->headline): ?>

<<?php echo $this->hl; ?>><?php echo $this->headline; ?></<?php echo $this->hl; ?>>
<?php endif; ?>

<?php
$this->import('FrontendUser', 'User');
$lastLogin = $this->User->lastLogin;
?>

<?php
$arrDocuments = array();
foreach ($this->categories as $category)
{
	if ($category->isReadableForCurrentMember() && $category->hasPublishedDocuments())
	{
		foreach ($category->documents as $document)
		{
			if ($document->isPublished())
			{
				$key = $document->getLastModificationTimestamp();
				while ($arrDocuments[$key] != null)
				{
					$key--;
				}
				$arrDocuments[$key] = $document;
			}
		}
	}
}
?>

<form action="<?php echo $this->action; ?>" method="post" id="listing" name="listing">
	<input type="hidden" name="FORM_SUBMIT" value="<?php echo $this->formId; ?>">
	<input type="hidden" name="REQUEST_TOKEN" value="{{request_token}}">
	<input type="hidden" name="docId" id="docId" value="">

	<?php if (count($this->messages['errors']) > 0): ?>
	<!-- Errors -->
	<div id="dms_errors">
		<?php foreach ($this->messages['errors'] as $error): ?>
		<div class="error"><?php echo $error; ?></div>
		<?php endforeach; ?>
	</div>
	<?php endif; ?>
	
	<?php if (count($this->messages['warnings']) > 0): ?>
	<!-- Warnings -->
	<div id="dms_warnings">
		<?php foreach ($this->messages['warnings'] as $warning): ?>
		<div class="warning"><?php echo $warning; ?></div>
		<?php endforeach; ?>
	</div>
	<?php endif; ?>
	
	<?php if (count($this->messages['successes']) > 0): ?>
	<!-- Successes -->
	<div id="dms_successes">
		<?php foreach ($this->messages['successes'] as $success): ?>
		<div class="success"><?php echo $success; ?></div>
		<?php endforeach; ?>
	</div>
	<?php endif; ?>
	
	<?php if (count($this->messages['infos']) > 0): ?>
	<!-- Infos -->
	<div id="dms_infos">
		<?php foreach ($this->messages['infos'] as $info): ?>
		<div class="info"><?php echo $info; ?></div>
		<?php endforeach; ?>
	</div>
	<?php endif; ?>
	
<?php krsort($arrDocuments); ?>
<?php $i = 0; ?>
<?php foreach ($arrDocuments as $document): ?>
	<?php if ($maxDocuments > 0 && $i >= $maxDocuments) break; ?>
	<div class="layout_simple block<?php if ($document->getLastModificationTimestamp() - $lastLogin > 0) : ?> new<?php endif; ?>">
		<?php echo date("d.m.Y", $document->getLastModificationTimestamp()); ?> 
	<?php
		$title = "Dokumentname: " . $document->name;
		$title .= "\nKategorie: " . implode(" &raquo; ", $document->category->getPathNames(false));
		$title .= "\n" . sprintf($GLOBALS['TL_LANG']['DMS']['listing_size'], $document->getFileSize(Document::FILE_SIZE_UNIT_MB, true));
		if (strlen($document->getUploadDate() > 0) && $document->hasUploadMemberName())
		{
			$title .= "\n" . sprintf($GLOBALS['TL_LANG']['DMS']['listing_uploaded'], $document->getUploadDate(), $document->uploadMemberName);
		}
		if (strlen($document->getLasteditDate() > 0) && $document->hasLasteditMemberName())
		{
			$title .= "\n" . sprintf($GLOBALS['TL_LANG']['DMS']['listing_lastedited'], $document->getLasteditDate(), $document->lasteditMemberName);
		}
		
		$docName = $document->name;
		if ($maxDocNameLength > 0 && strlen(utf8_decode($docName)) > $maxDocNameLength) {
			$docName = utf8_encode(substr(utf8_decode($docName), 0, $maxDocNameLength)) . ' ...';
		} 
	?>
		<a href="javascript:downloadFile('<?php echo $document->id; ?>')" title="<?php echo $title; ?>">
			<?php echo $docName; ?> <?php echo sprintf($GLOBALS['TL_LANG']['DMS']['listing_version'], $document->getVersion()); ?>
		</a>
		<small>(<?php echo ($document->hasLasteditMemberName()) ? $document->lasteditMemberName : $document->uploadMemberName;?>)</small>
		<span class="newIcon" title="Neu bzw. bearbeitet seit dem letzten Login">*</span> 
	</div>
	<?php $i++; ?>
<?php endforeach; ?>
</form>

<script type="text/javascript">
<!--
	function downloadFile (docId)
	{
		document.getElementById('docId').value = docId;
		document.getElementById('listing').submit();
	}
-->
</script>

</div>
